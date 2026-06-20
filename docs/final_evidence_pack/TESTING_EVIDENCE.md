# PulseGuard AI - Testing Evidence

## Test Suite Summary

| Suite | Framework | Tests | Files | Status |
|-------|-----------|-------|-------|--------|
| Backend | pytest | 110 | 22 | Available |
| Frontend | vitest + RTL | 54 | 14 | Available |
| Mobile | TypeScript | tsc --noEmit | — | Available |
| Load | k6 + Locust | 3 scenarios | 2 | Scripts available |
| CI | GitHub Actions | All above | 1 workflow | Configured |

## Backend Tests (22 files, ~3,463 lines)

Run with: `pytest backend/tests -v --tb=short`

| # | File | Tests | Component |
|---|------|-------|-----------|
| 1 | test_endpoints.py | Comprehensive | All API endpoints |
| 2 | test_anomaly_detection.py | Rule validation | WHO/AHA vital ranges, risk classification |
| 3 | test_ml.py | Model tests | ML model loading, risk/anomaly/intent inference |
| 4 | test_chatbot.py | Chatbot | Model loading, fallback, text cleanup |
| 5 | test_chat_safety.py | Safety | Disclaimer injection, emergency override |
| 6 | test_chat_firebase.py | Integration | Chat with Firebase telemetry context |
| 7 | test_assistant.py | NLU | Intent detection, response composition, memory |
| 8 | test_firebase_fallback.py | Fallback | In-memory store CRUD, caps |
| 9 | test_auth.py | Auth | Firebase token verification, Bearer parsing |
| 10 | test_auth_profile.py | Profile | Completeness, bootstrap, onboarding |
| 11 | test_contract.py | Contract | Telemetry schema, freshness thresholds |
| 12 | test_alert_engine.py | Alerts | Risk alerts, battery alerts, messages |
| 13 | test_rate_limit.py | Security | Rate limiter (429 responses) |
| 14 | test_admin_sdk.py | Firebase | Admin SDK initialization, error handling |
| 15 | test_freshness.py | Status | Device freshness (connected/stale/disconnected) |
| 16 | test_user_scoped.py | Isolation | Per-user data separation |
| 17 | test_vitals_api.py | API | Vitals normalization, derived fields |
| 18 | test_medical_slm.py | AI | Medical SLM demo mode, safe fallback |
| 19 | test_wesad_model_package.py | ML | WESAD model inference |
| 20 | test_signup_sensor_flow.py | E2E | signup -> bootstrap -> sensor data -> alerts |
| 21 | test_endpoints.py | Smoke | GET/POST all endpoints |
| 22 | conftest.py | Fixtures | App, client, mocked Firebase |

## Frontend Tests (14 files)

Run with: `npm test` (vitest)

| File | Component Tested |
|------|-----------------|
| Chat.test.tsx | Chat endpoint call, vitals context passing |
| Index.test.tsx | Dashboard rendering, stale state handling |
| Profile.test.tsx | Profile page rendering |
| useLiveTelemetry.test.ts | Polling, contract mapping, offline fallback |
| client.test.ts | Temperature normalization, telemetry schema |
| health-data.test.ts | Classification thresholds, F->C conversion |
| AlertSummary.test.tsx | Alert rendering |
| MetricCard.test.tsx | Metric card props |
| RiskHeroCard.test.tsx | Risk hero rendering |
| StatusBadge.test.tsx | Status badge states |
| HealthInsights.test.tsx | Health insight generation |
| TelemetrySourceBadge.test.tsx | Source badge states |
| ProactiveAlertCard.test.tsx | Alert deduplication |
| ErrorBoundary.test.tsx | Error boundary |

## CI Pipeline (.github/workflows/ci.yml)

**3 jobs run on every push/PR:**
1. Backend: Python 3.12, pip install, pytest
2. Frontend: Node 20, npm ci, eslint, vitest, vite build
3. Mobile: Node 20, npm install, tsc --noEmit

## Load Testing

| Script | Tool | Scenarios |
|--------|------|-----------|
| `load_tests/k6_backend_test.js` | k6 | 25-100 concurrent users, 60s |
| `load_tests/locustfile.py` | Locust | Python-based, configurable |

**Thresholds:**
- /health p95 < 200ms
- /api/latest p95 < 400ms
- /api/telemetry p95 < 800ms
- /api/chat p95 < 3000ms (rule-based)

## ML Model Evaluation Summary

| Model | Accuracy | F1 | AUC | Samples |
|-------|----------|-----|-----|---------|
| Risk Classifier | 99.24% | 0.99 | — | 9,000 test |
| Anomaly Detector | — | — | 0.80 | 10,000 test |
| Intent Classifier | 98.9% | 0.99 | — | 91 test |
| Activity (UCI HAR) | 96.2% | 0.96 | — | 2,947 test |
| WESAD Stress | 93.24% | 0.83 | 0.98 | 873 test |
| Stress (synthetic) | 95.61% | 0.96 | — | 1,800 test |
