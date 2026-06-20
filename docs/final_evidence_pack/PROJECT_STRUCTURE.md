# PulseGuard AI - Project Structure

```
grad_trial/
├── .env.example                          # Frontend env template
├── .github/workflows/ci.yml             # GitHub Actions CI (pytest + vitest + lint + build)
├── .gitignore                            # Excludes .env, secrets, node_modules, models
├── README.md                             # Main documentation (~390 lines)
├── package.json                          # Frontend: React 18, Vite 5, Tailwind, Firebase
├── package-lock.json
├── vite.config.ts                        # Dev server (port 8080), proxy /api -> :5000
├── tailwind.config.ts                    # Health color palette, custom animations
├── tsconfig.json / tsconfig.app.json / tsconfig.node.json
├── eslint.config.js
├── vitest.config.ts                      # Frontend test config
├── postcss.config.js
├── components.json                       # shadcn/ui config
├── index.html                            # SPA entry point
├── docker-compose.yml                    # 3 services: backend + frontend + simulator
├── Dockerfile.frontend                   # Nginx static build
├── nginx.conf                            # Reverse proxy config
├── firebase.rules.json                   # RTDB security rules (auth.uid === $uid)
│
├── src/                                  # React + Vite Web Dashboard
│   ├── main.tsx                          # React DOM render entry
│   ├── App.tsx                           # Routing + ProtectedRoute + profile gating
│   ├── App.css / index.css              # Tailwind imports
│   ├── vite-env.d.ts
│   ├── pages/
│   │   ├── Auth.tsx                      # Login / signup / demo mode
│   │   ├── Onboarding.tsx                # First-time profile setup (6 fields)
│   │   ├── Index.tsx                     # Dashboard: vitals, risk, chart, alerts
│   │   ├── Analytics.tsx                 # Trends, reports, CSV export
│   │   ├── Alerts.tsx                    # Current + historical alerts
│   │   ├── Chat.tsx                      # AI health assistant
│   │   ├── Profile.tsx                   # Profile edit, goals, photo upload
│   │   ├── NotFound.tsx                  # 404 page
│   │   └── *.test.tsx                    # Page tests
│   ├── components/
│   │   ├── MetricCard.tsx                # Vital display card (HR, SpO2, temp, etc.)
│   │   ├── RiskHeroCard.tsx              # Large risk level indicator + wellness score
│   │   ├── VitalsChart.tsx               # Heart rate trend (Recharts)
│   │   ├── HealthInsights.tsx            # Plain-language health recommendations
│   │   ├── ModelInsight.tsx              # ML model outputs display
│   │   ├── ProactiveAlertCard.tsx        # Anti-spam alert popup
│   │   ├── AlertSummary.tsx              # Compact alert strip
│   │   ├── TelemetrySourceBadge.tsx      # Connected/Stale/Disconnected badge
│   │   ├── StatusBadge.tsx               # Health status chip
│   │   ├── AppLayout.tsx                 # Sidebar + mobile nav
│   │   ├── ErrorBoundary.tsx             # React crash handler
│   │   ├── ui/                           # 50+ shadcn/ui Radix primitives
│   │   └── *.test.tsx                    # Component tests
│   ├── hooks/
│   │   ├── useAuth.tsx                   # Firebase Auth + demo mode
│   │   ├── useLiveTelemetry.ts           # Backend polling (1.5s vitals, 4.5s history)
│   │   ├── useHealthData.ts              # In-browser simulator fallback
│   │   ├── use-mobile.tsx                # Responsive breakpoint
│   │   └── use-toast.ts                  # Toast notification hook
│   ├── integrations/firebase/
│   │   ├── client.ts                     # Firebase init, fbPath helpers, type defs
│   │   └── client.test.ts
│   ├── lib/
│   │   ├── health-data.ts                # Classification, wellness score, data generation
│   │   ├── anomaly-detection.ts          # Z-score, IQR, moving average, isolation forest
│   │   ├── utils.ts                      # cn() helper (clsx + tailwind-merge)
│   │   └── health-data.test.ts
│   └── test/
│       ├── setup.ts                      # Vitest setup
│       └── example.test.ts
│
├── backend/                              # Flask Python Backend
│   ├── __init__.py
│   ├── app.py                            # Main Flask app (1670 lines, 33 endpoints)
│   ├── anomaly_detection.py              # Rule engine (WHO/AHA vital ranges)
│   ├── chatbot_service.py                # TinyLlama + PulseGuardAssistant wrapper
│   ├── firebase_service.py               # Firebase RTDB adapter (Admin SDK/REST/memory)
│   ├── llm_client.py                     # Multi-provider LLM client (Groq/OpenAI/Anthropic/Ollama)
│   ├── data_source.py                    # Firebase vs simulator resolver
│   ├── simulator.py                      # Synthetic telemetry generator (9 scenarios)
│   ├── alerts.py                         # Alert engine (current + historical)
│   ├── reports.py                        # Daily/weekly summaries + CSV export
│   ├── responses.py                      # Standard JSON envelope (ok/err)
│   ├── telemetry_contract.py             # Schema normalization + field mapping
│   ├── logging_config.py                 # Structured access logs + X-Request-ID
│   ├── clock.py                          # Time utilities
│   ├── .env.example                      # Backend env template (40+ variables)
│   ├── Dockerfile                        # Python backend container
│   ├── SETUP_WINDOWS.md                  # Windows-specific setup guide
│   ├── requirements.txt                  # Base: Flask, firebase-admin, scikit-learn
│   ├── requirements-ai.txt               # Optional: torch, transformers, peft, tensorflow
│   ├── requirements-dev.txt              # Dev: pytest, coverage
│   ├── ml/                               # ML Models
│   │   ├── __init__.py                   # Public API: get_models()
│   │   ├── registry.py                   # Lazy singleton model loader
│   │   ├── risk_classifier.py            # MLP (64-32-16) -> normal/warning/high
│   │   ├── anomaly_detector.py           # Autoencoder (6-4-2-4-6)
│   │   ├── intent_classifier.py          # TF-IDF + MLP -> 13 intents
│   │   ├── stress_classifier.py          # WESAD DeepDNN wrapper
│   │   ├── medical_slm.py               # TinyLlama/Phi-3 + LoRA medical model (~600 lines)
│   │   └── training/
│   │       ├── train_all.py              # One-command training orchestrator
│   │       ├── train_risk_classifier.py  # 60K synthetic samples, knowledge distillation
│   │       ├── train_anomaly_detector.py # 20K healthy-only samples
│   │       ├── train_intent_classifier.py # 515 hand-authored examples
│   │       ├── train_activity_classifier.py # UCI HAR dataset
│   │       ├── train_stress_classifier.py # Synthetic + optional WESAD
│   │       └── generate_dataset.py       # Synthetic data generation
│   ├── assistant/                        # Rule-based health assistant
│   │   ├── __init__.py
│   │   ├── assistant.py                  # PulseGuardAssistant orchestrator
│   │   ├── nlu.py                        # 18-intent regex NLU + metric aliases
│   │   ├── knowledge.py                  # Clinical knowledge base (AHA/WHO)
│   │   ├── responder.py                  # 5-step response composer
│   │   └── memory.py                     # Per-user conversation history
│   ├── models/                           # Saved model artifacts
│   │   ├── MODEL_CARDS.md                # Model documentation
│   │   ├── risk_classifier_metrics.json  # 99.24% accuracy
│   │   ├── anomaly_autoencoder_metrics.json # 0.80 AUC
│   │   ├── intent_classifier_metrics.json # 98.9% accuracy
│   │   ├── activity_classifier_metrics.json # 96.2% accuracy (UCI HAR)
│   │   ├── stress_classifier_metrics.json # 95.6% accuracy (synthetic)
│   │   ├── training_summary.json
│   │   ├── wesad_stress_comparison.json  # 7-model WESAD comparison
│   │   ├── wesad_stress_artifact.pkl     # WESAD pickle model
│   │   ├── medical_slm_adapter/          # TinyLlama LoRA (r=8, default)
│   │   ├── medical_phi3_lora_adapter/    # Phi-3 LoRA (r=16, optional)
│   │   └── wesad_vscode_model_package/   # DeepDNN stress model (93.2% acc)
│   │       ├── inference.py
│   │       ├── metadata.json             # 15-model bake-off results
│   │       ├── feature_names.json        # 252 features
│   │       ├── classification_report.txt
│   │       └── models/DeepDNN/           # Keras model + preprocessor
│   ├── tests/                            # 22 test files, ~3,463 lines
│   │   ├── conftest.py                   # Fixtures (app, client, mocks)
│   │   ├── test_endpoints.py             # All API endpoints
│   │   ├── test_anomaly_detection.py     # Rule engine
│   │   ├── test_ml.py                    # ML model loading
│   │   ├── test_chatbot.py               # Chatbot service
│   │   ├── test_auth.py                  # Firebase auth
│   │   └── ... (17 more test files)
│   └── scripts/
│       ├── check_firebase.py
│       ├── check_environment.py
│       ├── check_wesad_model.py
│       └── load_test_backend.py
│
├── mobile/                               # Expo React Native Mobile App
│   ├── app.json                          # Expo config (name, scheme, plugins)
│   ├── package.json                      # Expo SDK 51, React Native 0.74.5
│   ├── .env.example                      # EXPO_PUBLIC_API_BASE_URL, Firebase config
│   ├── tsconfig.json
│   ├── babel.config.js
│   ├── app/                              # Expo Router screens
│   │   ├── _layout.tsx                   # Root navigator
│   │   ├── index.tsx                     # Splash/redirect
│   │   ├── auth.tsx                      # Login + signup
│   │   ├── onboarding.tsx                # Profile setup
│   │   └── (tabs)/                       # Tab navigator
│   │       ├── _layout.tsx               # Tab bar config
│   │       ├── dashboard.tsx             # Live vitals
│   │       ├── alerts.tsx                # Alert feed
│   │       ├── chat.tsx                  # AI assistant
│   │       ├── history.tsx               # Reading history
│   │       ├── reports.tsx               # Daily/weekly summaries
│   │       └── profile.tsx               # User settings
│   └── src/
│       ├── config.ts                     # API URL, colors
│       ├── hooks/                        # useAuth, useLiveTelemetry, useAlertNotifications
│       └── lib/                          # api.ts, firebase.ts
│
├── firmware/                             # ESP32-C3 Reference Firmware
│   ├── platformio.ini                    # Board: esp32-c3-devkitm-1
│   ├── README.md
│   └── src/
│       ├── main.cpp                      # Sensor read + HTTP POST loop (178 lines)
│       └── config.h                      # WiFi, API URL, battery config
│
├── docs/                                 # 17 documentation files
│   ├── api.md                            # Full REST API reference
│   ├── openapi.yaml                      # OpenAPI 3.0 spec
│   ├── firebase.md                       # RTDB schema + rules
│   ├── deployment.md                     # Docker + local + production
│   ├── testing.md                        # Test catalog + coverage
│   ├── performance.md                    # Load testing
│   ├── security.md                       # Auth + validation + safety
│   ├── hardware.md                       # BOM + I2C map + battery
│   ├── ble_spec.md                       # GATT specification
│   ├── demo_script.md                    # 8-minute walkthrough
│   ├── final_defense_answers.md          # 22 Q&A
│   ├── team_contributions.md             # Role breakdown
│   ├── ai_production_checklist.md        # Healthcare AI safety
│   ├── FINAL_PROJECT_REPORT.md
│   └── ...
│
├── final_evidence/                       # Defense proof drawer
│   ├── architecture.md, api_endpoints.md, ai_components.md
│   ├── testing.md, load_testing.md, security.md, observability.md
│   ├── demo_script.md, setup_and_run.md
│   └── {api_tests, dashboard, firebase, load_tests, mobile, team}/
│
├── load_tests/                           # Performance tests
│   ├── k6_backend_test.js                # k6 (25-100 users)
│   └── locustfile.py                     # Locust (Python)
│
└── public/                               # Static assets
    ├── logo.png, favicon.ico
    ├── manifest.webmanifest              # PWA manifest
    ├── robots.txt
    └── sw.js                             # Service worker (network-first for /api/)
```
