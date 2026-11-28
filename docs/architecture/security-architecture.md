# Security architecture

Step 3.7 approved; controls not yet implemented or tested.

## Identity and sessions
One initial Admin account, no registration or role master. Provision securely using backend command; no hardcoded password. Argon2id minimum m=19456 KiB, t=2, p=1, per-password random salt; benchmark. Password length 15–128, spaces/special chars allowed, reject common/compromised values, no unjustified periodic reset. Verify current password before change. Never return password or hash. MFA/recovery decision before public release.

Persistent MariaDB-backed Express Session, no production MemoryStore, no JWT. Session cookie, HttpOnly, SameSite=Lax, host-only, path `/`; Secure in production HTTPS only. Idle and absolute timeouts are configured and enforced server-side. Regenerate session after login; revoke sessions after password change; destroy session and clear cookie on logout; clean expired rows. Frontend clears user and sensitive query cache on logout/session expiry. No tokens in localStorage/sessionStorage. Login failures generic.

## CSRF and request protection
Session-bound token from GET `/api/v1/auth/csrf-token`; send `X-CSRF-Token` on state-changing operations including login/logout; keep in memory, refresh after session regeneration. 403 `CSRF_INVALID`; no silent replay of financial writes. Verify trusted Origin/Referer as defense in depth. GET does not mutate business records. Same-origin proxy preferred; no unrestricted CORS. Only configured reverse proxy trusted. Helmet, CSP at actual serving layer, HSTS in production, MIME/frame/referrer protection, no sensitive caching or stack traces, request body size limits.

Configurable per-IP and per-account limits cover general API, login failures, password changes, SMTP tests, PDF generation and financial actions. Operational values are maintained in private configuration. Temporary restrictions, avoid permanent single-admin lockout; rate limit 429 with Retry-After. Review effectiveness and proxy trust.

## Data, secrets and documents
Restricted dev/test runtime accounts and distinct migration privileges; no root for Express, no global database changes. Parameterized SQL, allowlisted dynamic identifiers, transaction locking and unique constraints. No public DB, phpMyAdmin or internal API port. Server-only `.env` and production secret store; separate session/encryption keys; SMTP AES-256-GCM or protected secret reference, never return existing password. Verify SMTP TLS certificate; avoid arbitrary outbound destinations, validate SMTP configuration against approved policy. IRP credentials server-only. Pino redacts secrets, cookies, auth headers and sensitive bodies; append-only application audit events with actor/action/entity/time/request ID, redacted details. Audit and financial rows not deletable through normal APIs.

PNG/JPEG logo/signature, configured size limit; verify file contents and dimensions, generated storage identifiers, no executable uploads. Private file storage outside public roots; authenticated downloads, PDF from immutable snapshot, file hash. Protect CSV exports against formula injection. Do not commit storage, logs, DB dumps, keys or credentials. Financial idempotency and concurrency controls required; external SMTP/IRP retries reconcile uncertain outcomes.

## Repository and production
Public repository with company authorization dated 28 November 2026. Authorization excludes confidential information, credentials and sensitive infrastructure configuration. Follow the mandatory publication gate in `docs/development-workflow/publication-security.md`. Individual accounts, MFA, PR review, CI, branch protections on main/develop, no force push/deletion; enable supported secret scanning/dependency alerts. Rotate accidentally exposed secrets.

Production release requires supported database/runtime, HTTPS, restricted admin access, private storage, isolated environments, approved backup/restore and security tests. Recovery targets and backup schedules are maintained in the private operational register; verify that backup frequency and transaction-log recovery meet the approved RPO. Do not claim achieved until tested. Use synthetic data locally. Security verification includes unauthorized routes, CSRF, session expiry/revocation, injection, upload/download checks, idempotency, concurrent financial operations, redacted logs and recovery.
