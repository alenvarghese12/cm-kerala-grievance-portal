# CM Kerala – Public Grievance Redressal Portal

A full-stack recruitment prototype built with React + Vite, Python FastAPI, MySQL and Groq LLM API. This is a fictional demonstration project and is not an official Government of Kerala or Chief Minister's Office service.

## Features
- Public pages: Home, About, How it Works, Departments, Contact, FAQ
- Citizen registration/login, grievance submission, uploads, reference ID, tracking, dashboard and feedback/reopen
- CMO dashboard, status statistics, filters, department assignment, officer assignment support, remarks and escalation through status handling
- Department officer view, investigation/action-taken report submission to CMO review
- Groq AI analysis for department/category/priority/summary; labelled heuristic fallback when Groq is not configured
- Status history with timestamps and actor IDs
- MySQL persistence, JWT authentication and role-based access
- FastAPI interactive docs at `/docs`

## Requirements
- Python 3.11+
- Node.js 20.19+ or 22.12+
- MySQL Server 8+
- MySQL Workbench (optional GUI)

## 1. Database setup (MySQL Workbench)
Open Workbench, connect to your local MySQL server and run:

```sql
CREATE DATABASE cm_kerala_portal CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

## 2. Backend setup (Windows PowerShell)
```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
copy .env.example .env
```
Edit `backend/.env` and set your actual MySQL password in `DATABASE_URL`. Example:
`mysql+pymysql://root:YourPassword@localhost:3306/cm_kerala_portal`

Set a long random `SECRET_KEY`. Add your Groq key to `GROQ_API_KEY` (get it from https://console.groq.com/keys). Do not commit `.env`.

Start API:
```powershell
uvicorn app.main:app --reload
```
Open `http://localhost:8000/docs` and `http://localhost:8000/api/v1/health`.
Tables and sample departments/demo users are created automatically at first startup.

## 3. Frontend setup
Open a second terminal:
```powershell
cd frontend
npm install
copy .env.example .env
npm run dev
```
Open `http://localhost:5173`.

## Demo accounts (local development only)
- Citizen: `citizen@demo.local` / `Citizen@123`
- CMO admin: `cmo@demo.local` / `Cmo@12345`
- PWD officer: `officer@demo.local` / `Officer@123`

Change/remove demo credentials before public deployment. Create additional department officer users through database administration or a future admin provisioning endpoint. The demo officer is attached to the first seeded department (PWD).

## Main workflow
1. Register/login as citizen.
2. Submit grievance; optionally click AI analysis first.
3. Login as CMO admin, open grievance, assign department and move status as needed.
4. Login as matching department officer, open grievance, submit action report to CMO (`Under Review`).
5. CMO reviews and marks `Resolved`.
6. Citizen tracks reference number, submits feedback, or reopens a resolved grievance.

## API overview
- `POST /api/v1/auth/register`, `POST /api/v1/auth/login`, `GET /api/v1/auth/me`
- `GET /api/v1/departments`
- `POST /api/v1/ai/analyse`
- `POST /api/v1/grievances`, `GET /api/v1/grievances`, `GET /api/v1/grievances/{id}`
- `GET /api/v1/grievances/track/{reference_no}`
- `POST /api/v1/grievances/{id}/assign`
- `POST /api/v1/grievances/{id}/status`
- `POST /api/v1/grievances/{id}/escalate` (CMO)
- `POST /api/v1/grievances/{id}/remarks`
- `GET /api/v1/cmo/statistics`, `GET /api/v1/officers`
- `POST /api/v1/grievances/{id}/feedback`
- `POST /api/v1/contact`, `GET /api/v1/cmo/enquiries`

## Security and limitations
- Passwords are hashed with Argon2; JWTs are required for protected routes.
- Backend enforces citizen, CMO and officer role restrictions.
- Uploads are limited by size and extension; tune MIME/content scanning for production.
- Set exact frontend origins in `FRONTEND_ORIGINS`; do not use wildcard CORS in production.
- For production, use HTTPS, managed secrets, rate limiting, audit logging, malware scanning, backups, and private object storage.
- The AI endpoint treats complaint text as untrusted input and asks Groq for structured JSON. Validate all AI output and require human review. The fallback is explicitly not an LLM integration.
- The current prototype does not send email/SMS notifications. Updates are visible in the portal and tracking page.
- Public tracking reveals limited status information by reference number; use an additional verification factor before production use.
- Officer accounts should be provisioned securely by an administrator. Do not expose public officer registration.

## Deployment
- Frontend: deploy `frontend` to Vercel; set `VITE_API_URL` to the deployed API base URL.
- Backend: deploy `backend` to Render/Railway or another Python host; configure all environment variables and persistent upload storage.
- Database: use a reachable managed MySQL instance; allow the backend host network access.
- Set `FRONTEND_ORIGINS` to your exact deployed frontend origin.
- Keep `SECRET_KEY` and `GROQ_API_KEY` private. Never commit `.env`.
- Confirm `/api/v1/health`, `/docs`, login, file uploads and complete role workflow after deployment.

## Documentation
See `DOCUMENTATION/` for project overview, architecture, database, API, AI integration, setup/testing and user guide.
