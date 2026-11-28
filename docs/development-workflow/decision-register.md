# Decision register and open gates

Date: 28 November 2026. Approved: Step 3.1 scope, 3.2 stack, 3.3 architecture, 3.4 database standards, 3.5 API standards, 3.6 UI standards, 3.7 security, 3.8 roadmap. Step 3.9 documentation pending review/authorized commit and merge; Phase 4/5 workflow details pending; P01 not authorized.

| Open item | Required before | Owner |
|---|---|---|
| Public repository authorization confirmed; security verification and owner approval of sanitized changes | Every commit and push | Both / owner |
| Branch protection and reviewer rules | Enforced shared workflow; manual review meanwhile | Company administrator |
| Node runtime alignment, package versions and lockfile layout | P01 dependency installation/lock | Both |
| Approve exact system health/readiness contract | P01 implementation/integration | Both |
| ITSAZZ reference, logo, palette and design verification | Final frontend visual approval | Frontend developer / owner |
| Column-level types, nullability, constraints and indexes | Each migration | Backend developer |
| Invoice starting number, FY/series, historical continuity/migration decision | Numbering and issuance | Owner / Backend developer |
| Company tax registration, GST/export/LUT, IRN/IRP, FX and rounding | Applicable tax/issuance implementation and go-live | Owner / CA |
| PDF renderer, branded A4 layout and SMTP provider | P07 | Both / owner |
| Billing anchors/month-end, advance/arrears, partial periods/proration, paused/missed periods, draft discard and renewal rules | P08/P09 | Owner / both |
| TDS evidence, over-settlement and credits against paid invoices | P10/P11 | Owner / CA |
| MFA/recovery and production hosting, supported DB/runtime, backup/retention/RPO/RTO | Production | Owner / administrator / both |

Do not invent unresolved financial rules. Gate only dependent work; unrelated approved work may proceed once authorized. No external DB or changes to XAMPP authorized. Detailed contracts and SQL migrations not yet created beyond proposed system contract documentation.
