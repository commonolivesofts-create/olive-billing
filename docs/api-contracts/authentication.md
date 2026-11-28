# Authentication contract baseline

Step 3.5/3.7 approved architecture. Detailed P02 contract pending; no endpoint implemented.

| Method | Endpoint | Purpose |
|---|---|---|
| GET | /api/v1/auth/csrf-token | Session-bound CSRF token before login and after session rotation |
| POST | /api/v1/auth/login | Email and password, establishes HttpOnly cookie |
| GET | /api/v1/auth/me | Current authenticated user |
| POST | /api/v1/auth/logout | Destroy session |
| POST | /api/v1/auth/change-password | Current and new password, revoke sessions |

Login example request `{ "email": "admin@example.com", "password": "<password>" }`. Proposed success data `{ "user": { "id": "1", "name": "Administrator", "email": "admin@example.com" } }`, in standard envelope; no password/hash/token returned. Other exact request fields and CSRF response shape pending P02 approval. No registration. See security architecture for cookie/CSRF/timeout controls. Frontend handles 401 session expiry, clears sensitive cache; 403 CSRF/permission errors; 429 temporary limits; never automatically replay sensitive writes. Production HTTPS only.
