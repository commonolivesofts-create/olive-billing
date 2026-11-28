# Git and review workflow

Existing repository: https://github.com/commonolivesofts-create/olive-billing.git. Setup complete; last verified baseline 03f2ccac84186a15a4a46314ef20004ab1c67494; both computers clean on develop at verification. Do not reinitialize or clone again. This document records the established strategy; Phase 4/5 detailed workflow still to be approved.

`main` stable releases, `develop` integrated, `feature/backend-*`, `feature/frontend-*`, `feature/integration-*`. Feature branches from latest develop; first planned `feature/backend-p01-foundation`, `feature/frontend-p01-foundation`. Review shared docs and contract, develop in assigned directories, stage only intended files, test, push branch, PR to develop with independent review, verify integrated build, delete short-lived branch after merge. Release PR develop to main after approval. No direct feature pushes to permanent branches or force push. Shared files require coordination. No credentials/private docs in public repo. Company has authorized public publication of sanitized material only; mandatory security gate before every commit and push; branch protections pending; manual independent review until enforced. CI lint/typecheck/test/build and isolated DB tests planned, never use live data.

Cross-chat workflow: Development Plan and Git Strategy central/backend; Project Continuation Planning frontend. Send status update after each approved step/task; repository docs are durable reference after authorized merge. No assumption of automatic monitoring. Acknowledge contract version, raise mismatches, block affected task until corrected; mocks dev/test only.

See `docs/development-workflow/publication-security.md`. Do not publish private configuration, operational procedures or real data.
