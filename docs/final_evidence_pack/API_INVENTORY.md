# PulseGuard AI - Complete API Inventory

All endpoints defined in `backend/app.py`. Response envelope: `{ok: bool, data: {...}, message: string}` or `{ok: false, error: {code, message}}`.

## Endpoints (33 total)

| # | Method | Path | Purpose | Rate Limit | Line |
|---|--------|------|---------|------------|------|
| 1 | GET | `/` | Root banner + links | default | 485 |
| 2 | GET | `/health` | Liveness (version, uptime, firebase_mode, services) | default | 498 |
| 3 | GET | `/api/health` | Alias for /health | default | 499 |
| 4 | GET | `/api/metrics` | In-process counters (requests, telemetry, alerts, chat) | default | 680 |
| 5 | GET | `/api/models` | Loaded ML model info + LLM provider status | default | 520 |
| 6 | POST | `/api/telemetry` | Ingest reading, validate, analyze, persist, alert | 60/min | 786 |
| 7 | GET | `/api/latest` | Raw latest telemetry for user | default | 859 |
| 8 | GET | `/api/vitals/latest` | Normalized current state (single source of truth) | default | 867 |
| 9 | GET | `/api/vitals/history` | Normalized history (?limit=200, max 1000) | default | 888 |
| 10 | GET | `/api/vitals/window` | Aggregated recent window (?seconds=60) | default | 904 |
| 11 | GET | `/api/device/status` | Bracelet connectivity + freshness | default | 954 |
| 12 | GET | `/api/history` | Back-compat history (?limit=100) | default | 987 |
| 13 | GET | `/api/alerts/current` | Current alerts only (live state) | default | 1020 |
| 14 | GET | `/api/alerts` | Current + historical alerts | default | 1036 |
| 15 | GET | `/api/alerts/stored` | Raw alert log (?limit=50, max 500) | default | 1056 |
| 16 | GET | `/api/reports/daily` | Daily health summary (last 24h) | default | 1084 |
| 17 | GET | `/api/reports/weekly` | Weekly health summary (last 7 days) | default | 1406 |
| 18 | GET | `/api/reports/export.csv` | CSV download (?limit=1000, max 5000) | default | 1410 |
| 19 | POST | `/api/ml/predict/stress` | WESAD stress prediction (features/vector input) | default | 558 |
| 20 | POST | `/api/simulate` | Generate synthetic reading + ingest (?mode=) | 30/min | 1433 |
| 21 | GET | `/api/simulate/modes` | List simulator scenarios | default | 1428 |
| 22 | POST | `/api/chat` | Chatbot reply with telemetry context | 30/min | 1531 |
| 23 | POST | `/chat` | Back-compat alias for /api/chat | 30/min | 1536 |
| 24 | POST | `/ai/medical-slm` | Medical LoRA model Q&A | default | 595 |
| 25 | GET | `/api/profile` | User profile from Firebase | default | 1103 |
| 26 | GET | `/api/goals` | User goals from Firebase | default | 1109 |
| 27 | POST | `/api/auth/bootstrap` | Create profile + goals on signup | default | 1115 |
| 28 | GET | `/api/me` | User profile + completeness (Bearer token) | default | 1302 |
| 29 | PUT | `/api/profile/me` | Update profile / onboarding (Bearer token) | default | 1329 |
| 30 | GET | `/api/auth/bootstrap/check` | Debug: check profile/goals existence | default | 1387 |
| 31 | POST | `/update_telemetry` | Legacy Arduino bridge (sensor data ingest) | 60/min | 1548 |
| 32 | GET | `/latest` | Legacy: read internal sensor cache | default | 1667 |
| 33 | GET | `/api/models/status` | Alias for /api/models | default | 521 |

## Key Endpoint Details

### POST /update_telemetry (Sensor Data Ingest)

**Request (from ESP32/Arduino):**
```json
{
  "heart_rate": 72,
  "spo2": 98,
  "temperature": 98.6,
  "steps": 0,
  "sleep_duration": 12,
  "battery_level": 83,
  "fall_alert": false
}
```

**Processing:**
1. Parse JSON (force=True for missing Content-Type)
2. Map fields (temperature F->C, field name aliases)
3. analyze() -> risk_level, wellness_score, activity, stress
4. ML enrichment (risk classifier, anomaly detector)
5. UID resolution (explicit -> device_pairing -> frontend_uid -> env)
6. Firebase write (latest + history + alerts)
7. Return `{status: "success"}`

### GET /api/vitals/latest

**Response:**
```json
{
  "ok": true,
  "data": {
    "available": true,
    "heart_rate": 72.0,
    "spo2": 98,
    "temperature_c": 37.0,
    "steps": 0,
    "battery_level": 83,
    "wellness_score": 100,
    "risk_level": "normal",
    "stress_label": "relaxed",
    "activity": "resting",
    "device_status": "connected",
    "last_seen_seconds": 0.5,
    "source": "firebase",
    "is_simulated": false,
    "timestamp": 1781944000000,
    "uid": "user-uid-here"
  }
}
```

### POST /api/chat

**Request:**
```json
{
  "message": "How is my heart rate?",
  "uid": "user-uid"
}
```

**Response:**
```json
{
  "ok": true,
  "data": {
    "response": "Your heart rate is 72 bpm, which is in the normal range...",
    "intent": "metric_query",
    "source": "pulseguard_ai",
    "latency_ms": 2,
    "suggestions": ["What about my SpO2?", "Any tips for better sleep?"]
  }
}
```

### Simulator Modes (GET /api/simulate/modes)
`resting`, `walking`, `running`, `sleep`, `fever`, `high_fever`, `stress`, `anomaly`, `low_battery`
