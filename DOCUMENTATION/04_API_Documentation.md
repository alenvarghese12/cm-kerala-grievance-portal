# 04 – API Documentation

Base URL: `http://localhost:8000/api/v1`. Interactive OpenAPI documentation: `/docs`.

| Method | Endpoint | Access | Purpose |
|---|---|---|---|
| GET | `/health` | Public | Database health |
| POST | `/auth/register` | Public | Citizen registration |
| POST | `/auth/login` | Public | Login and JWT |
| GET | `/auth/me` | Authenticated | Current user |
| GET | `/departments` | Public | Departments |
| POST | `/ai/analyse` | Authenticated | Groq analysis/fallback |
| POST | `/grievances` | Citizen | Submit multipart grievance |
| GET | `/grievances` | Authenticated | Role-filtered list |
| GET | `/grievances/{id}` | Authorized role | Detail |
| GET | `/grievances/track/{reference_no}` | Public | Limited tracking |
| POST | `/grievances/{id}/assign` | CMO | Assign department/officer |
| POST | `/grievances/{id}/status` | Role-based | Status transition |
| POST | `/grievances/{id}/escalate` | CMO | Mark high priority and record escalation |
| POST | `/grievances/{id}/remarks` | CMO/officer | Add internal remarks |
| GET | `/cmo/statistics` | CMO | Counts and grouped statistics |
| GET | `/officers` | CMO | Officer list |
| POST | `/grievances/{id}/feedback` | Citizen | Post-resolution feedback |
| POST | `/contact` | Public | Save contact enquiry |
| GET | `/cmo/enquiries` | CMO | View enquiries |

## Login request
```json
{"email":"citizen@demo.local","password":"Citizen@123"}
```

## AI request
```json
{"description":"The village road has dangerous potholes","subject":"Damaged road","district":"Kollam"}
```

## Assignment request
```json
{"department_id":1,"officer_id":3,"remarks":"Please inspect this location"}
```

## Status request
```json
{"status":"Under Review","action_report":"Inspection completed","resolution":"Repair recommended","remarks":"Sent to CMO"}
```

For multipart grievance submission, send `district`, `category`, `subject`, `description`, `priority`, `ai_suggestion` and zero or more `files`.
