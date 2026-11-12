# 05 – AI Integration

## Provider
Groq API, called server-side using the official `groq` Python SDK. Default model: `llama-3.3-70b-versatile`; set `GROQ_MODEL` to a currently available model in your Groq account.

## Workflow
1. Citizen enters complaint text.
2. Frontend sends text to `POST /api/v1/ai/analyse`.
3. FastAPI calls Groq with constrained JSON instructions.
4. Response is parsed and priority is validated.
5. Department, category, priority and summary are returned for citizen/CMO review.
6. The user may edit category and decide whether to submit.

## Safeguards
- API key remains on backend.
- User complaint is treated as untrusted input.
- AI suggestions are not official decisions.
- Human review is required.
- Invalid or unavailable LLM responses return a clearly labelled heuristic fallback.
- Fallback is not claimed as completed LLM integration.

## Configuration
Set `GROQ_API_KEY` and `GROQ_MODEL` in backend `.env`. Obtain credentials from https://console.groq.com/keys. Do not commit keys.

## Limitations
LLM output can be inaccurate or inconsistent. No duplicate matching is implemented as a production similarity engine; the model may optionally return duplicate terms. Add reviewed duplicate detection and model evaluation before production.
