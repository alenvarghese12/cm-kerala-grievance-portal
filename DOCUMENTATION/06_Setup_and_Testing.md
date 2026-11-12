# 06 – Setup and Testing

See root README for full Windows setup.

## Local verification
1. MySQL database `cm_kerala_portal` exists.
2. Backend virtual environment is active and requirements installed.
3. Backend `.env` has a valid DATABASE_URL.
4. Start `uvicorn app.main:app --reload` from `backend`.
5. Verify `http://localhost:8000/api/v1/health`.
6. Start Vite using `npm run dev` from `frontend`.

## Manual test cases
- Register a citizen with a unique email.
- Log in and check `/auth/me`.
- Run AI analysis with Groq configured; verify provider is Groq and fallback false.
- Run without Groq key; verify response explicitly labels fallback.
- Submit grievance and verify reference number.
- Track reference number without login.
- Log in as CMO; assign department; update status and inspect history.
- Log in as department officer; submit action report as Under Review.
- Log in as CMO; mark Resolved.
- Log in as citizen; submit feedback or reopen.
- Submit contact enquiry and view it as CMO.
- Verify citizen cannot access another citizen's grievance and officer cannot access another department's record.

## Automated tests
The API is structured for HTTPX/Pytest. Configure a dedicated test database before adding integration tests. Never run destructive tests against production.
