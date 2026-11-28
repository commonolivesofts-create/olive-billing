# Core relationships

Conceptual ERD; exact constraints and introduction order reviewed with migrations.
```mermaid
erDiagram
  CUSTOMERS ||--o{ INVOICES : billed
  CUSTOMERS ||--o{ BILLING_PLANS : contracts
  TAX_RATES ||--o{ SERVICES : default_tax
  BILLING_PLANS ||--o{ BILLING_PLAN_LINES : includes
  BILLING_PLANS ||--o{ BILLING_CYCLES : schedules
  BILLING_PLANS ||--o{ BILLING_PLAN_EVENTS : records
  BILLING_CYCLES o|--o| INVOICES : generates
  INVOICES ||--o{ INVOICE_LINES : includes
  INVOICES ||--o{ INVOICE_PAYMENTS : settles
  INVOICES ||--o{ ADJUSTMENT_NOTES : corrects
  ADJUSTMENT_NOTES ||--o{ ADJUSTMENT_NOTE_LINES : includes
```
Nullable links: invoices.billing_plan_id and source_invoice_id; billing_plans.predecessor_plan_id; billing_cycles.invoice_id unique if populated; optional previous_invoice_reference. Service references and bank references as defined in source; preserve snapshots. Document, email and IRN owner is exactly one invoice or adjustment note; enforce ownership and linkage in DB and service. No circular DDL dependency: defer references to later-created tables until both exist.
