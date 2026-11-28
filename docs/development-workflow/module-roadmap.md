# Implementation roadmap

Step 3.8 approved. P00 is planning (Steps 3.1–3.9 plus Phase 4/5 workflow sign-off), P01–P13 implementation and release. All 29 MVP entries mapped. No actual implementation yet.

| Phase | Activity | Requirement | Dependencies |
|---|---|---|---|
| P00 | Planning | — | — |
| P01 | Application foundation | 2 | P00 |
| P02 | Authentication and security | 1 | P01 |
| P03 | Company and billing settings | 3–7 | P02 |
| P04 | Master data | 8–10 | P03 |
| P05 | Invoice drafts and calculations | 11–14 | P04 |
| P06 | Invoice issuance and IRN | 15–16 | P05 |
| P07 | PDF and email | 17–18 | P06 |
| P08 | Recurring billing plans | 19–20 | P07 |
| P09 | Billing cycles and continuity | 21–23 | P08 |
| P10 | Payments and outstanding | 24–25 | P06, P09 |
| P11 | Credit/debit notes | 26–27 | P06, P07, P10 |
| P12 | Dashboard and history | 28–29 | P09–P11 |
| P13 | Production release | All 29 | P00–P12 |

Milestones: P07 one-time invoice/PDF/email; P09 recurring billing and continuity; P12 complete settlement and reporting; P13 production acceptance. P01 first tasks BE-01.01 and FE-01.01; no coding until explicitly authorized. Exact subsequent task IDs and module contracts to be reviewed at each phase. A phase is accepted only after actual API integration, tests and reviewed merges; design phase uses document approval. No delivery estimates assigned.

## Gate by phase
P01 fresh setup/build, isolated migrations, health/readiness; P02 login/CSRF/session tests; P03 company configuration, numbering and email settings; P04 master validity and duplicate prevention; P05 draft editor, server decimal/tax calculations; P06 numbering/immutable snapshots, applicable IRN and cancellation; P07 private PDF and controlled email retry; P08 plan schedule validation; P09 unique cycle, missed period recovery and renewal history; P10 settlement and reversal; P11 linked corrections and balance; P12 reconciled currency-separated summaries and export; P13 regression, security, backup restore, UAT, production configuration. Conditional statutory cases cannot be marked accepted without approved treatment.
