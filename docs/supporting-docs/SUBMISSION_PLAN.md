# Course submission checklist

The release repository is https://github.com/aokamutidao/agent-arena-escrow, branch `main`.

| Required deliverable | Current evidence / final action |
| --- | --- |
| 10–15 page technical report | Final report assembled from requirements, architecture, test records and actual deployment facts |
| Five design diagrams | Architecture, contract interaction, data model, lifecycle/process and UI navigation |
| Source repository and setup | Independent main repository and setup instructions; [four CI jobs passed](https://github.com/aokamutidao/agent-arena-escrow/actions/runs/38061976954); document component reuse and individual contributions |
| Public URL and contract details | Both Render services live on main 9e8e429; public page/API/chain smoke passed; Sepolia deployment JSON records five addresses and receipts |
| Testing and impact evaluation | TEST_REPORT.md, contract reports, local transaction/evidence JSON and scenario evaluation |
| Slides and presentation video | 14-slide deck and a presentation video (about 15 minutes) delivered separately; the embedded tournament 13 demonstration and its script/validation are complete |
| User manual | USER_GUIDE.md and the separately formatted submission manual |
| Individual contributions | Use the student's confirmed identity and actual work attribution; distinguish reused prototype work from course changes |

## Verified deployment references

- Frontend: https://agent-arena-course-web.onrender.com
- API: https://agent-arena-course-api.onrender.com
- Chain: Sepolia 11155111
- Manager: `0x4EAFee134156c65ae64887B5e7B861977C4CFa98`
- Deployment block: `11885235`
- [Contract addresses, receipts and checks](evidence/sepolia-deployment-2026-10-10.json)

## Evaluation scope

The coursework uses scenario-based evaluation: exact funding and payout balances, independent review, immutable evidence, refund conservation, restart recovery, gas scaling and bounded local query measurements. Stakeholder interviews and satisfaction surveys are outside the completed evaluation. The completed [public smoke check](evidence/cloud-smoke-2026-10-10.json) covers page delivery, HTTPS, API and chain configuration, language switching and the installed MetaMask connection entry. It performed no wallet signing or financial transaction; the complete real-model financial workflow is demonstrated locally. [CI evidence](evidence/github-ci-2026-10-10.json) records four successful jobs on main 9e8e429.

## Final packaging

Public URL and CI verification are complete. Final exports are complete: technical report 15 pages, presentation 14 slides, user manual 5 pages, deployment/testing record 3 pages, contribution template 1 page, and the presentation video (about 15 minutes) built around the 600-second recorded demonstration. Complete the remaining identity/contribution fields with student-confirmed information. Include the report PDF, presentation, user manual, contribution statement, deployment/testing attachment and video or its accepted sharing link. Source and documentation remain in Git; private configuration, original recordings, local chain state and generated runtime files do not.
