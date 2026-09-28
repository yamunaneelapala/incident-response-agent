# SignalOps Incident Response

A local Incident Response console. Open `frontend/index.html` directly in Chrome or Edge; the static frontend needs no Node.js, npm, Vite server, or running backend. It includes a local demo sign-in, sample incidents, incident analysis, response recommendations, and an incident-aware chatbot. Changes and conversations are saved in that browser's local storage.

**Safety boundary:** the assistant has no production tool access and cannot execute commands. The human approval control records that an operator reviewed a response-plan step; it does not run a production action.

## Frontend

Double-click `frontend/index.html`, or open that file in Chrome or Edge. Sign in with `jordan.davis@example.test` / `signalops-demo`, or choose **Continue with demo workspace**. The prototype login gate is stored in this browser and is not production authentication; passwords are not transmitted or stored. Sign out from the profile menu. Incidents remain saved separately in this browser and are not deleted when signing out.

## Optional Backend

The FastAPI backend remains available for API-based workflows. Backend setup requires Python 3.10 or newer. To install its dependencies from the project root:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r backend\requirements.txt
```

To enable LLM analysis in the backend, copy `.env.example` to `.env` and set `LLM_API_KEY`. `LLM_BASE_URL` and `LLM_MODEL` default to OpenAI's chat completions API and `gpt-4o-mini`. Keep `.env` private.

```powershell
python -m uvicorn backend.main:app --reload --host 127.0.0.1 --port 8000
```

The backend API docs are at <http://localhost:8000/docs>. SQLite is created at `backend/incidents.db` and seeded on first startup. The standalone `frontend/index.html` app does not require or contact this API.

## Included workflows

- Dashboard totals, analytics, service health, and an AI incident brief across active and resolved incidents.
- Incident creation with title, description, affected service, severity, status, responder/team, impact, and supporting signals.
- Incident detail with possible causes, evidence, impact, confidence, recommended next steps, assignments, and a timestamped activity timeline.
- Incident history search and dynamic filters for status, severity, service, and date.
- Ask SignalOps workspace chat and an incident-aware copilot grounded in saved incident data.
- AI-recommended response options with simulation-only controls; no command execution or production changes.
- Unread notifications for critical incidents, service degradation, completed analysis, and resolution.
- Reports workspace to browse reports for all incidents, with impact, timeline, actions, resolution, and recommendations; copy or print/save as PDF.
- Guided demo mode that simulates detection, investigation, analysis, assignment, recommendation, and resolution.
- On-call roster, response policies, and editable hindsight reviews.

## REST endpoints

- `GET /api/health`, `GET /api/dashboard`
- `POST /api/analyze` to classify an incident and return recommended response steps without saving it
- `GET /api/incidents?search=&status=&severity=&type=`
- `POST /api/incidents`, `GET /api/incidents/{id}`
- `PATCH /api/incidents/{id}/status`
- `POST /api/incidents/{id}/approve`
- `POST /api/incidents/{id}/chat`

CORS allows the default Vite origins at `localhost:5173` and `127.0.0.1:5173`.