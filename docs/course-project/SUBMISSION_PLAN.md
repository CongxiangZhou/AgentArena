# Course submission checklist

The release repository is https://github.com/aokamutidao/agent-arena-escrow, branch `main`.

| Required deliverable | Current evidence / final action |
| --- | --- |
| 10–15 page technical report | Final report assembled from requirements, architecture, test records and actual deployment facts |
| Five design diagrams | Architecture, contract interaction, data model, lifecycle/process and UI navigation |
| Source repository and setup | Independent course repository; root README, Dockerfiles and CI; preserve truthful reuse and contribution attribution |
| Public URL and contract details | Render frontend/API URLs in DEPLOYMENT.md; Sepolia deployment JSON with five addresses and receipts; final public smoke check pending |
| Testing and impact evaluation | TEST_REPORT.md, contract reports, local transaction/evidence JSON and scenario evaluation |
| Slides and ten-minute demonstration | Presentation delivered separately; final 600-second video and its script/validation are complete |
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

The coursework uses scenario-based evaluation: exact funding and payout balances, independent review, immutable evidence, refund conservation, restart recovery, gas scaling and bounded local query measurements. No stakeholder interview or satisfaction survey is fabricated. The public smoke check covers page delivery, HTTPS, API and chain configuration; the complete real-model financial workflow is demonstrated locally.

## Final packaging

Confirm all URLs are reachable and all placeholders are replaced. Include the report PDF, presentation, user manual, contribution statement, deployment/testing attachment and video or its accepted sharing link. Source and documentation remain in Git; private configuration, original recordings, local chain state and generated runtime files do not.
