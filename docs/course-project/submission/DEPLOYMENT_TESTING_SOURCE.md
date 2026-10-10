# Agent Arena Deployment and Testing Record

SC 6113 | Release evidence | 10 October 2026

## Public release

Source repository: https://github.com/aokamutidao/agent-arena-escrow. Public application: https://agent-arena-course-web.onrender.com. Public API: https://agent-arena-course-api.onrender.com. Both Render services are live. Six HTTP smoke checks passed; browser checks confirmed the dashboard, language switch, chain sync and installed MetaMask entry. Public execution uses deterministic simulation; the recorded local match uses real model calls. Public network: Sepolia (11155111). Manager contract: 0x4eafee134156c65ae64887b5e7b861977c4cfa98. The main release commit 9e8e429 passed all four GitHub Actions jobs: frontend, backend, contracts and integration.

The independent course repository uses `main`. It identifies the course entry point and reused game components; earlier development history remains in the original working repository. Contract addresses appear on the following page.

## Deployment acceptance scope

The public presentation instance is configured for deterministic simulation. The recorded local demonstration uses real `qwen3.5-plus` calls. The locally authorised model credential is not included in the public release or transferred into cloud configuration.

The six passing HTTP checks cover the public page, health, readiness, configuration, tournament list and dashboard. Readiness, Sepolia configuration and frontend CORS passed. Wallet-entry inspection did not sign in or send a transaction. Contract receipts, deployed code, relationships and roles were checked separately. No public event, mint, sponsorship, match or payout was executed. The full workflow and real-model run are evidenced locally.

## Reproduction and operating assumptions

The repository includes builds, CI, configuration examples and deployment instructions. The backend discovers its token and child contracts from the manager. Keep chain ID, manager, deployment block and database identity aligned. `/health` checks liveness; `/ready` checks deployment and indexing.

The free Render tier can spin down on inactivity, take time to wake, and discard changes on the local filesystem. SQLite is therefore transient in this presentation environment. On-chain projections can be rebuilt, but original Agent definitions, match evidence and review notes need retained copies. The submitted local evidence package preserves the actual demonstration. A durable service would use persistent storage and tested backups.

## Evidence locations

`docs/course-project/evidence/sepolia-deployment-2026-10-10.json` records receipts and contract reads; `cloud-smoke-2026-10-10.json` in the same directory records the public acceptance. `docs/course-project/submission/SUBMISSION_FACTS.json` records release URLs and acceptance scope. `docs/course-project/DEPLOYMENT.md` contains setup and operation guidance. Full test records are indexed by `docs/course-project/TEST_REPORT.md`.

<!-- PAGE -->
# 1 Sepolia contract deployment

Network: Sepolia, chain ID 11155111. Compiler: Solidity 0.8.24, optimiser enabled with 200 runs, Cancun EVM target. Original contract source revision: `79bc8235d1b5e74758da087a14739c9474f3de76`; the independent course snapshot publishes that implementation on main. Manager deployment block: 11885235. Token deployment block: 11885234.

| Contract | Deployed address |
| --- | --- |
| TournamentManager | 0x4EAFee134156c65ae64887B5e7B861977C4CFa98 |
| TournamentRegistry | 0xA376e469BbfBB1c68E8c8940071EE20a3c816bD4 |
| PrizeEscrow | 0x40939074Ebcb4967080D924119cEceb4081Eb02D |
| ResultReview | 0xA2002AF04fb7Be169a0044BAcdB18C2F68291038 |
| CourseToken | 0x36600325144a43e00A7664E7e7C0d4dDa54955f4 |

## Successful transactions

Token deployment at block 11885234, gas used 4,045,496:

`0xb5fe69bf0d3de104b30c201db4848e9edb527a88b782e237a4c4c7ec263cf02d`

Manager and component deployment at block 11885235, gas used 39,663,436:

`0x987895b8f5716aab5c2974702499cbc4265b11b150400f47f1e673e81a9fe890`

Organiser role grant at block 11885236, gas used 133,537:

`0x6896e4a78fd81a5de3fba3a6be51df4b85dd920723edf5b5a2398e671c944b7a`

Auditor role grant at block 11885237, gas used 133,537:

`0xf787d7d8910dda383cfe7ff81dd124c0a3782f69b8b43b7a46f35d04949f0baf`

Each transaction can be inspected at `https://sepolia.etherscan.io/tx/` followed by its hash. Explorer links for every receipt are included in the JSON evidence. Deployed bytecode is present for all five addresses, the manager's settlement token and child modules match the listed addresses, and the administrator, organiser and auditor role reads returned true.

At verification, DUSD reported six decimals and total supply zero, and the next tournament ID was one. The deployment created no prize pool or participant balance. An earlier reverted deployment attempt with gas used equal to its supplied limit is retained separately in the evidence and is not counted as a successful deployment.

<!-- PAGE -->
# 2 Automated and scenario testing

## Published continuous integration

The published main commit `9e8e429` completed four GitHub Actions jobs successfully: frontend, backend, contracts and integration. Run evidence: https://github.com/aokamutidao/agent-arena-escrow/actions/runs/38061976954

This actual remote run is preserved in `docs/course-project/evidence/github-ci-2026-10-10.json`. Later documentation-only commits do not change the tested code revision. The CI configuration is in `.github/workflows/course-ci.yml`.

| Evidence | Result | Interpretation |
| --- | --- | --- |
| Foundry suite | 49 passed | 41 course tests plus 8 legacy regressions |
| Stateful invariant | 64 runs at depth 64 | 4,096 actions under the retained test configuration |
| Funds fuzz tests | 256 runs each | Two independent fuzz tests |
| Manager coverage | Lines 98.59%; branches 92.50% | Module coverage; whole-repo line coverage is 80.54% |
| Go service | Test, vet, race and build passed | Dated raw logs retained in evidence directory |
| Frontend local final suite | 42 tests passed | Includes account, session, language and integer-money regressions |
| MetaMask event 7 | 9 successful receipts | Separate recorded wallet acceptance, not cloud execution |
| User video event 13 | 100 DUSD paid to blue | 15 rounds and 30 successful real-model calls |

## Financial and evidence checks

The deterministic local integration exercises sponsorship, registration, result submission, review, prize claim, dispute cancellation and refund. It checks actual ERC20 balances, zero final liability, nonce replay rejection, unauthorised execution rejection, evidence hashes, duplicate-event behaviour and backend restart. A subsequent real-model CLI scenario records a 100 DUSD prize and an independent 25 DUSD refund. The final video concentrates on the main prize workflow.

Gas scale snapshots compare a single claim with 1, 16 and 64 sponsors. Measured claimPrize is 74,024 gas in each case, and claimRefund is 71,635 in each case. These controlled call measurements differ from complete transaction receipt costs. Local API timings use only 30 sequential loopback samples per endpoint and are not a cloud load test.

The tests do not establish an external security audit, production service-level objective or independent survey result. The verification scope is explicit so the evaluator can reproduce each claimed result from the supplied command, report or receipt.
