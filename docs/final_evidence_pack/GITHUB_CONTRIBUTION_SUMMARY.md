# PulseGuard AI - GitHub & Repository Summary

## Repository Info
- **Main branch**: main
- **CI**: GitHub Actions (pytest + vitest + lint + build + tsc)
- **Platform**: GitHub (private repository)

## Team Contributions
(Source: `docs/team_contributions.md`)

| Member | Program | Primary Contributions |
|--------|---------|----------------------|
| **Omar Medhat** | DSAI | AI chatbot (TinyLlama + PEFT LoRA), Flask backend architecture, ML model loading with safety guardrails, fallback composer, health insight generation |
| **Asmaa Desokey** | DSAI | Health-data simulation (clinical distributions), rule-based anomaly detection (Z-score, IQR, moving average), WESAD stress model, patient scenarios |
| **Lama Omar** | Software Engineering | React + Vite dashboard, shadcn UI components, Firebase Authentication, mobile app UI, frontend-backend integration |

**Cross-cutting (all three):**
- System architecture + defense materials
- Firebase RTDB schema + security rules
- Testing suites (pytest, vitest, manual mobile)
- Documentation (17 files)
- Docker deployment
- GitHub Actions CI

## Repository Professionalism Checklist

| Item | Status | Evidence |
|------|--------|---------|
| README.md | YES | ~390 lines, comprehensive |
| Setup instructions | YES | README + docs/deployment.md + SETUP_WINDOWS.md |
| .env.example | YES | Root + backend + mobile |
| .gitignore excludes secrets | YES | .env, serviceAccountKey.json |
| requirements.txt | YES | Base + AI + dev |
| package.json | YES | Root + mobile |
| API documentation | YES | docs/api.md + openapi.yaml |
| Database schema | YES | docs/firebase.md + firebase.rules.json |
| Tests exist | YES | 110 backend + 54 frontend |
| CI configured | YES | .github/workflows/ci.yml |
| Docker configured | YES | docker-compose.yml |
| Documentation | YES | 17 files in docs/ |
| Screenshots | YES | final_evidence/ subfolders |

## Files Modified During Merge Session (2026-06-20)

Files transferred from second project:
- `backend/models/stress_classifier_metrics.json`
- `backend/models/wesad_stress_artifact.pkl`
- `backend/models/wesad_stress_comparison.json`
- `backend/ml/training/train_stress_classifier.py`
- `wesad_dataset_latest_version.py`

Files created:
- `.env` (root + backend)
- `mobile/.env`

Files modified:
- `backend/app.py` — added `force=True` for JSON parsing, added `_last_frontend_uid` to UID resolution, added debug logging for sensor data
- `vite.config.ts` — changed proxy target to port 5000
- `src/pages/Chat.tsx` — changed endpoint from `/ai/medical-slm` to `/api/chat`
- `src/components/ModelInsight.tsx` — removed architecture labels from UI
