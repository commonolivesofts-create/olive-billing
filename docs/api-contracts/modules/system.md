# P01 foundation API contract — proposed v0.1

Review required before implementation. Owners: Backend developer (backend), Frontend developer (frontend reviewer). BE-01.09 / FE-01.08. No business data, database details, secrets or version strings in response. Both GET endpoints have no request body/query, no authentication or CSRF requirement for local foundation probes; restrict readiness at production proxy/monitoring boundary. No caching. Request ID in envelope and proposed X-Request-Id response header.

## GET /api/v1/health
Liveness only; does not query database. HTTP 200:
```json
{"success":true,"message":"Application is healthy","data":{"status":"healthy"},"requestId":"example-request-id"}
```
No HTTP response is possible if server is stopped; client must handle network failure.

## GET /api/v1/ready
Checks required configured dependencies, including a bounded MariaDB query and migration readiness when available. HTTP 200:
```json
{"success":true,"message":"Application is ready","data":{"status":"ready"},"requestId":"example-request-id"}
```
HTTP 503 when required dependencies are unavailable or required schema is not ready:
```json
{"success":false,"message":"Application is not ready","data":null,"error":{"code":"SERVICE_UNAVAILABLE","details":[]},"requestId":"example-request-id"}
```
Unexpected error follows standard HTTP 500 envelope; do not reveal connection credentials or internal diagnostics. Acceptance: live process with DB unavailable still returns health 200; ready 503; healthy DB and applied required migrations return ready 200; both responses match schema; frontend handles network errors, no fake readiness. Approve precise schema before integration. Implementation occurs in P01, not this documentation step.
