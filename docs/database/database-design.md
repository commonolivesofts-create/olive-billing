# Database design and standards

Step 3.4 approved; conceptual schema, not executable DDL. Workbook field inventory in [data dictionary](data-dictionary.md).

Existing XAMPP MariaDB, InnoDB and utf8mb4. No global changes. Development and test schemas and restricted runtime accounts are separate; the migration identity is separately scoped. Connection values use server-only environment variables. Production requires a supported database; no real financial data in an unsupported local stack.

## Table inventory
| Table | First phase |
|---|---|
| `users` | P02 |
| `sessions` | P02 |
| `audit_logs` | P02 |
| `company_settings` | P03 |
| `billing_settings` | P03 |
| `document_sequences` | P03 |
| `bank_accounts` | P03 |
| `tax_settings` | P03 |
| `email_settings` | P03 |
| `customers` | P04 |
| `tax_rates` | P04 |
| `services` | P04 |
| `invoices` | P05 |
| `invoice_lines` | P05 |
| `idempotency_requests` | P06 |
| `invoice_irn` | P06 |
| `invoice_documents` | P07 |
| `invoice_emails` | P07 |
| `billing_plans` | P08 |
| `billing_plan_lines` | P08 |
| `billing_cycles` | P09 |
| `billing_plan_events` | P09 |
| `invoice_payments` | P10 |
| `adjustment_notes` | P11 |
| `adjustment_note_lines` | P11 |
| `schema_migrations` | P01 |

26 tables: 22 functional tables plus sessions, audit_logs, idempotency_requests and schema_migrations. No separate reports/dashboard tables. Introduce dependencies in order, add later foreign keys only after referenced tables exist. The numbering, IRN and document ownership rules must remain valid across invoices and notes.

## Naming and types
Lowercase snake_case tables/columns, plural table names, `id` PK, `<entity>_id` FK; `created_at`, `updated_at` on stored tables; `uq_`, `fk_`, `idx_`, `chk_` names. BIGINT UNSIGNED AUTO_INCREMENT business IDs, corresponding FKs; technical natural keys (e.g. session ID or migration version) retain required uniqueness. Monetary DECIMAL(20,6), quantity DECIMAL(18,6), tax rate DECIMAL(9,6), FX DECIMAL(20,10); currency CHAR(3), DATE business dates, DATETIME(3) UTC events, TINYINT(1) booleans, VARCHAR(30) validated status, validated JSON-compatible text for snapshots. Exact per-column nullability, length, indexes and constraints approved with each migration. DB/API IDs and decimals serialized as strings. Strict SQL mode on app connections, UTC timestamps explicitly configured.

## Invariants
Unique normalized admin email, customer code, service code; unique FY/document series/number, numbering only on issue with cancelled numbers retained, GST series max 16 characters. Single default bank. Restrictive FKs, immutable historical snapshots and documents. Unique (billing_plan_id, period_start), unique cycle invoice link. Exactly one invoice or adjustment-note owner for IRN, PDF and email records. Idempotency scope/key uniqueness with payload validation. ETag/version on applicable mutable financial records.

Financial writes lock the relevant rows and use transactions; balance = issued invoice + issued debit notes - issued credit notes - active receipts - active TDS. Reject excess settlement; no advance or multi-invoice allocation. Currency-specific reporting, no cross-currency totals without approved conversion. Overdue = due date passed and positive balance. No hard deletion of issued documents or posted receipts; reversal preserves history. IRN applicability, rounding, credit limits, FX and recurrence edge cases require approved rules. Drafts generated only, advance schedule only after successful draft creation. External operations need reconciliation rather than assuming database rollback reverses them.
