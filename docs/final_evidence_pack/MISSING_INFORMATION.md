# PulseGuard AI - Missing Information for Final Deliverables

## HIGH PRIORITY (Required for final report submission)

| # | Item | Where Needed | How to Get |
|---|------|-------------|-----------|
| 1 | **Student IDs** | Title page, all deliverables | Team members provide their university IDs |
| 2 | **Supervisor name + title** | Title page, acknowledgments | Confirm with department |
| 3 | **Team number** | Title page | Confirm with department |
| 4 | **Final project title** (exact spelling) | All deliverables | Confirm: "PulseGuard AI - Smart Health Monitoring Bracelet" |
| 5 | **GitHub repository URL** | README, poster, report | Copy from GitHub settings |
| 6 | **Screenshots of running app** | Report, presentation, poster | Capture manually (see SCREENSHOT_INDEX.md) |
| 7 | **Hardware photos** | Report hardware section, poster | Photo of ESP32 + sensors + bracelet prototype |

## MEDIUM PRIORITY (Improves quality of deliverables)

| # | Item | Where Needed | How to Get |
|---|------|-------------|-----------|
| 8 | **Demo video recording** | Presentation, poster QR code | Screen record 3-4 min demo flow (see docs/demo_script.md) |
| 9 | **Firebase Console screenshot** (redacted) | Database section | Screenshot RTDB tree in Firebase Console |
| 10 | **Serial Monitor screenshot** | Hardware section | Capture Arduino IDE serial monitor with sensor readings |
| 11 | **Test run output** | Testing section | Run `pytest backend/tests -v` and save output |
| 12 | **Frontend build verification** | Testing section | Run `npm run build` and save output |
| 13 | **Training loss curves** | ML section | Extract from CHATBOT_FINAL_VERSION.ipynb or re-run training |
| 14 | **Confusion matrix visualizations** | ML section | Generate from metrics JSON files |
| 15 | **Final deployment URL** (if deployed) | Poster, report | Deploy to Render/Cloud Run/etc. |

## LOW PRIORITY (Nice to have)

| # | Item | Where Needed | How to Get |
|---|------|-------------|-----------|
| 16 | **Formal literature review** | Report chapter 2 | Research related work in health monitoring |
| 17 | **IEEE/ACM citation formatting** | Report references | Format using Zotero/Mendeley |
| 18 | **Poster QR code** | Poster | Generate QR for demo video or GitHub URL |
| 19 | **BLE GATT specification** (if BLE implemented) | Hardware section | Currently WiFi only; BLE spec in docs/ble_spec.md is reference |
| 20 | **User study / usability feedback** | Evaluation section | Conduct with 3-5 users if time permits |
| 21 | **License file** | Repository | Add MIT or Apache 2.0 license |
| 22 | **Code coverage report** | Testing section | Run `pytest --cov=backend` and `npm run test:coverage` |
| 23 | **Performance benchmark results** | Performance section | Run k6/Locust load tests and save results |

## NOTES

- The WESAD DeepDNN model (93.2% accuracy) is trained and available but requires TensorFlow (not installed by default). This is a deliberate design choice to keep the default install lightweight.
- The Medical SLM (TinyLlama + LoRA) requires ~4GB RAM on CPU. For demos, use `MEDICAL_SLM_DEMO_MODE=1` for instant safe fallback responses.
- The Activity Classifier (96.2% on UCI HAR) metrics are available but the model is not loaded at runtime (offline-trained, metrics displayed in /api/models).
- ML model .joblib files are excluded from git (.gitignore). They must be generated via `python -m backend.ml.training.train_all` after cloning.
