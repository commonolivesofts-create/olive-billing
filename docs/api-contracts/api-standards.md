# Shared REST API standards

Approved Step 3.5. Contract prefix `/api/v1`; relative client URLs through Vite proxy in development and same-origin reverse proxy in production. JSON except file transfer; camelCase payload, kebab-case resource URLs, snake_case database. GET reads; POST creates/actions; PATCH partial updates; PUT replacement where specified; DELETE only if explicitly permitted. No hard deletion of issued/posted records.

Success: `{ "success": true, "message": "...", "data": {}, "requestId": "..." }`. Error: `{ "success": false, "message": "...", "data": null, "error": { "code": "VALIDATION_ERROR", "details": [{"field":"customerName","message":"Required"}] }, "requestId": "..." }`. `error.details` array (empty for general errors); stable error codes, safe message, no stack/SQL/secrets. 204 has no body. Request ID correlates sanitized logs. Standard response header `X-Request-Id` proposed in foundation contract. HTTP 200/201/204 for success; 400 malformed, 401 authentication, 403 forbidden/CSRF, 404 absent, 409 conflict, 412 version conflict, 422 validation, 429 limit, 500 unexpected, 503 unavailable. Never 200 for error.

## Lists
page default 1, limit default 25/max 100, search, sortBy, sortOrder asc/desc and approved filters. Server allowlists sort/filter and stable tie-break. Data array plus meta.pagination `{page,limit,totalRecords,totalPages,hasNextPage,hasPreviousPage}`. Individual contracts define empty-page behavior. IDs strings; quantity, money, tax and FX decimal strings (no scientific notation for financial payloads); date YYYY-MM-DD; event UTC ISO timestamp; true booleans; nullable only by contract. PATCH omitted = unchanged; null = clear only if allowed.

## Security and consistency
Server-side sessions, HttpOnly cookie, state-changing X-CSRF-Token. `Idempotency-Key` on issuance, cancellation, payment record/reverse, note issue and recurring generation; same key for same logical retry, different payload conflicts. Record replay and business mutation atomically where possible; external calls reconciled. ETag/If-Match on mutable financial records; stale version 412 VERSION_CONFLICT. Required precondition behavior finalized per module. Backend authorizes every protected action and calculates final amounts.

## File transfers
Uploads multipart/form-data with verified size/content/type, private storage. PDF application/pdf, approved CSV/XLSX export, standard JSON errors. Authenticated downloads only. OpenAPI / Swagger planned at `/openapi.json` and `/api-docs` restricted in production. No endpoint is implemented yet.

## Contract checklist
Requirement, version, method/path, auth/headers, path/query/body schemas, data types/nullability, business and validation rules, HTTP status/error codes, response examples, concurrency/idempotency, tests and handoff status. Both developers approve changes before integration. Do not guess field names or use production mocks.
