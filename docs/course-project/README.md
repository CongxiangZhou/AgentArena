# Course documentation

Agent Arena is an AI tournament prize escrow, review and settlement DApp. The independent release repository is https://github.com/aokamutidao/agent-arena-escrow on `main`.

## Read in this order

1. [Requirements and acceptance](REQUIREMENTS.md)
2. [Architecture, process and data model](ARCHITECTURE.md)
3. [Contract responsibilities and 13 transaction types](CONTRACT.md)
4. [REST API and authentication](API.md)
5. [User manual](USER_GUIDE.md) and [first local walkthrough](EXPERIENCE_GUIDE.md)
6. [Local setup, Render deployment and recovery](DEPLOYMENT.md)
7. [Testing and measured results](TEST_REPORT.md)
8. [Code learning guide](LEARNING_GUIDE.md)
9. [Submission checklist](SUBMISSION_PLAN.md)
10. [Final recorded demonstration](demo-user-recording/README.md)

## Release evidence

- [Sepolia deployment](evidence/sepolia-deployment-2026-10-10.json): token, four interacting course contracts, successful receipts and role checks. Manager deployment block: 11885235.
- [Local wallet acceptance](evidence/metamask-main-flow-2026-10-10.json): tournament 7, nine successful transactions and a verified 100 DUSD prize.
- [Local real-model and refund integration](evidence/local-llm-integration-2026-10-10.json): tournament 5 uses real model decisions; tournament 6 verifies a 25 DUSD refund through the local test runner.
- [Final video validation](demo-user-recording/VALIDATION.json): tournament 13; 600 seconds, 1080p, English voiceover and bilingual subtitles.

The public frontend is https://agent-arena-course-web.onrender.com and the API is https://agent-arena-course-api.onrender.com. Both services are live on release `9e8e429`. The [cloud smoke check](evidence/cloud-smoke-2026-10-10.json) passed for HTTPS, the page, API health/readiness, Sepolia configuration and synced data; Chrome language switching and the installed MetaMask entry also passed. No cloud signature or financial transaction was executed. All four [main-branch CI jobs](https://github.com/aokamutidao/agent-arena-escrow/actions/runs/38061976954) passed; see the [CI record](evidence/github-ci-2026-10-10.json). Cloud configuration uses Sepolia and labelled simulation; the complete real-model and financial demonstration remains local.

## Scope and provenance

This release contains the current code and its evidence. Historical planning logs and the previous development history remain in the original development workspace. Legacy engine and Agent code are retained for reuse and regression tests; the course entry point is `backend/cmd/course-server`.

Documentation separates implementation, local verification, public deployment and real-world impact. No user survey, real-money operation or production availability claim is inferred from passing code tests.

## Submission manuscripts

The [English report and supporting documents](submission/README.md) include five diagrams and verified deployment facts. [Presentation notes](submission-slides/NOTES.en.md) accompany the 12-slide deck. PDF, editable documents and the 10-minute video are included in the separate submission package.
