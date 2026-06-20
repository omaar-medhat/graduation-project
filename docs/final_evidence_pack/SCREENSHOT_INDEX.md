# PulseGuard AI - Screenshot Capture Checklist

Save all screenshots to: `docs/final_evidence_pack/screenshots/`

## Required Screenshots

| # | Filename | Page/State | How to Capture | Deliverable Section |
|---|----------|-----------|---------------|-------------------|
| 1 | `01_login.png` | Auth page (sign in form) | Open `http://localhost:8081/auth` | User Guide, Final Report |
| 2 | `02_signup.png` | Auth page (sign up form) | Click "Create an account" | User Guide |
| 3 | `03_onboarding.png` | Onboarding form | Create new account, fills 6 fields | User Guide, Final Report |
| 4 | `04_dashboard_normal.png` | Dashboard with normal vitals, sensor connected | Login with sensor running | Final Report, Poster, Presentation |
| 5 | `05_dashboard_warning.png` | Dashboard with warning/high risk | Simulate: `POST /api/simulate {"mode":"fever"}` | Final Report, Testing |
| 6 | `06_dashboard_metrics.png` | Metric cards close-up (HR, SpO2, Temp, Steps) | Scroll down on dashboard | Final Report |
| 7 | `07_vitals_chart.png` | Heart rate trend chart | Dashboard with history data | Final Report, Analytics |
| 8 | `08_analytics.png` | Analytics page with charts | Navigate to `/analytics` | Final Report |
| 9 | `09_analytics_report.png` | Daily/weekly report view | Click "Daily" on analytics | Final Report |
| 10 | `10_alerts.png` | Alerts page (current + historical) | Navigate to `/alerts` | Final Report |
| 11 | `11_chatbot_question.png` | Chat: user asks about vitals | Ask "How are my vitals?" on `/chat` | Final Report, Demo |
| 12 | `12_chatbot_response.png` | Chat: AI responds with vitals context | Capture response with source/latency | Final Report, AI Section |
| 13 | `13_profile.png` | Profile page with user data | Navigate to `/profile` | User Guide |
| 14 | `14_profile_goals.png` | Goals section (steps, sleep, calories) | Scroll on `/profile` | User Guide |
| 15 | `15_backend_terminal.png` | Backend running with logs | Terminal: `python -m backend.app` | Testing, Deployment |
| 16 | `16_sensor_logs.png` | Sensor POST success logs | Backend log: `POST /update_telemetry status=200` | Hardware, Testing |
| 17 | `17_firebase_data.png` | Firebase Console RTDB view (redacted) | Firebase Console -> RTDB | Database Section |
| 18 | `18_mobile_login.png` | Mobile app login screen | Expo Go app | Mobile Section |
| 19 | `19_mobile_dashboard.png` | Mobile app dashboard | Expo Go after login | Mobile Section, Poster |
| 20 | `20_mobile_chat.png` | Mobile app chat | Expo Go chat tab | Mobile Section |
| 21 | `21_hardware_photo.png` | Physical bracelet/hardware | Take photo of ESP32 + sensors | Hardware Section |
| 22 | `22_serial_monitor.png` | Arduino Serial Monitor output | PlatformIO serial monitor | Hardware, Testing |
| 23 | `23_model_insight.png` | ML model insight card on dashboard | Dashboard Neural Network section | AI Section |
| 24 | `24_risk_hero_card.png` | Risk hero card (All Good / Warning / High) | Dashboard top section | Final Report |

## Notes
- Redact any real passwords, tokens, or private keys in screenshots
- Use demo mode or test accounts for all screenshots
- Capture at 1920x1080 or higher resolution
- Include browser URL bar for web screenshots
- Include phone status bar for mobile screenshots
