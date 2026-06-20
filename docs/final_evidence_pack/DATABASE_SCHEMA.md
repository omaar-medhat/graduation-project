# PulseGuard AI - Firebase Database Schema

## Services Used
- **Firebase Realtime Database** (RTDB) - primary data store
- **Firebase Authentication** - Email/Password sign-in
- NO Firestore, NO Cloud Storage, NO Cloud Functions

## Database Tree

```
/
├── latest_telemetry              # Root: real bracelet's live reading (world-readable)
├── history/{push_id}             # Root: timestamped readings (world-readable)
│
└── users/{uid}/                  # User-scoped (auth.uid === $uid)
    ├── latest_telemetry          # Current reading
    │   ├── heart_rate: number            # 20-250 bpm
    │   ├── spo2: number                  # 50-100 %
    │   ├── temperature_c: number         # 25-45 C
    │   ├── steps: number                 # 0+
    │   ├── calories: number              # 0+
    │   ├── sleep_duration_sec: number    # 0-86400
    │   ├── battery_level: number         # 0-100 %
    │   ├── wellness_score: number        # 0-100
    │   ├── activity: string              # resting|active|walking|running
    │   ├── stress_label: string          # relaxed|normal|stressed
    │   ├── stress_score: number          # 0-100
    │   ├── risk_level: string            # normal|warning|high
    │   ├── alert_message: string
    │   ├── source: string                # real_bracelet|simulator
    │   └── timestamp: number             # ms epoch
    │
    ├── history/{push_id}         # Append-only log (capped 500)
    │   └── (same fields as latest_telemetry)
    │
    ├── alerts/{alert_id}         # Risk alerts (capped 100)
    │   ├── risk_level: string            # warning|high
    │   ├── message: string
    │   ├── reasons: object               # {hr_high: true, spo2_low: true, ...}
    │   ├── source: string                # rule_engine|device
    │   └── timestamp: number
    │
    ├── profile                   # User demographics
    │   ├── uid: string
    │   ├── email: string
    │   ├── name: string
    │   ├── age: number                   # 1-120
    │   ├── gender: string                # male|female|other
    │   ├── height_cm: number
    │   ├── weight_kg: number
    │   ├── activity: string              # sedentary|light|moderate|active|very_active
    │   ├── blood_type: string            # A+|A-|B+|B-|AB+|AB-|O+|O-
    │   ├── emergency_contact: string
    │   ├── photo: string                 # base64 dataURL
    │   ├── profile_complete: boolean
    │   ├── onboarding_completed: boolean
    │   ├── created_at: string            # ISO timestamp
    │   └── updated_at: string            # ISO timestamp
    │
    └── goals                     # Health targets
        ├── steps: number                 # default 10000
        ├── calories: number              # default 500
        └── sleep: number                 # default 8 (hours)
```

## Security Rules (firebase.rules.json)

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        ".write": "auth != null && auth.uid === $uid",
        "latest_telemetry": {
          ".validate": "newData.hasChildren(['heart_rate', 'spo2', 'temperature_c'])"
        }
      }
    },
    "latest_telemetry": { ".read": true, ".write": true },
    "history": { ".read": true, ".write": true }
  }
}
```

## Read/Write by Module

| Path | Written By | Read By |
|------|-----------|---------|
| `users/{uid}/latest_telemetry` | Backend (POST /update_telemetry, POST /api/telemetry) | Backend (GET /api/vitals/latest), Frontend (polling) |
| `users/{uid}/history` | Backend (on every telemetry ingest) | Backend (GET /api/vitals/history, reports) |
| `users/{uid}/alerts` | Backend (when risk != normal) | Backend (GET /api/alerts) |
| `users/{uid}/profile` | Frontend (onboarding, profile edit), Backend (bootstrap) | Frontend (profile check), Backend (GET /api/profile) |
| `users/{uid}/goals` | Frontend (onboarding, profile), Backend (bootstrap) | Backend (GET /api/goals) |
| Root `latest_telemetry` | Backend (legacy root write) | Backend (legacy read) |

## Firebase Modes in Backend

| Mode | Condition | R/W | File:Line |
|------|-----------|-----|-----------|
| `admin_sdk` | `FIREBASE_CREDENTIALS_PATH` valid | Full R/W | `firebase_service.py:244` |
| `rest` | URL only, no credentials | Read-only | `firebase_service.py:222` |
| `memory` | No URL, no credentials | In-memory | `firebase_service.py:194` |
