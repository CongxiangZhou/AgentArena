# Course release status

Release repository: https://github.com/aokamutidao/agent-arena-escrow. Default branch: `main`. Updated 10 October 2026.

| Area | Status and evidence |
| --- | --- |
| Financial workflow | Local wallet creation, sponsorship, registration, AI match, review and 100 DUSD claim verified |
| Real LLM | Tournament 13 recording: 15 rounds and 30 successful calls; earlier receipt-verified tournament 7 retained |
| Refunds | Local integration verified; final video shows historical refund records without another execution |
| Interface | Responsive English/Chinese UI; account-switch and session restoration regression tests |
| Tests | 49 contracts (41 course + 8 legacy), 42 frontend; Go test/race/vet and local integration records |
| Gas scaling | Current committed snapshots: prize 74,024 and refund 71,635 gas for 1, 16 and 64 sponsors |
| Video | Completed 600-second 1080p narrated demonstration with bilingual subtitles |
| Sepolia | Five contracts deployed, roles initialised, code/relationship reads verified |
| Public services | API health/readiness/chain checks passed; frontend build and page check pending |
| Submission materials | Report, slides, user manual and contribution statement assembled separately for final release |

Details: [tests](course-project/TEST_REPORT.md), [deployment](course-project/DEPLOYMENT.md), [submission checklist](course-project/SUBMISSION_PLAN.md), [video](course-project/demo-user-recording/README.md).

The independent repository starts with a course release snapshot. It does not recreate or invent the original development commit history. Future RAG, tool use and memory enhancements are outside this release.
