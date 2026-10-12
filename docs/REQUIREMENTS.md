# Course Edition Requirements and Acceptance Criteria

## Problem and Users

Small AI tournaments involve organisers, sponsors, participating developers and auditors. The problem we are validating is: whether the prize pool is secured in advance, whether the prize-claiming rules are fixed in advance, whether results and payments are traceable, and how sponsor funds are returned after a tournament is cancelled or stalls.

Public background material: FACEIT's prize process requires teams to submit a split plan, allows a dispute waiting period, and uses batch payments. Its documentation targets processing within less than 45 days after verification and split information are submitted, with some cases possibly taking longer.
Source: https://support.faceit.com/hc/en-us/articles/9718204637596-How-do-I-claim-a-cash-prize-such-as-a-League-or-Cash-Cup-prize (consulted: 2026-10-09).

This material only supports the claim that "tournament prize settlement involves real complexity". It cannot prove that this project has solved the needs of real organisations, nor can identity verification, tax and dispute waiting periods all be attributed to technology. The formal report needs to add interviews or structured user testing with target organisers/developers.

## Scope of This Round

- One tournament corresponds to two registered wallets and their immutable Agent definitions.
- The organiser creates the tournament; any wallet that meets the rules can sponsor. Registration is free of charge.
- The prize pool is pre-funded before the tournament starts; the winner receives the entire sponsored prize pool of the tournament, with no platform fee.
- Registration has a deadline; results have a review/dispute window and a final settlement deadline.
- The organiser submits the match result; a designated independent auditor approves or rules; the organiser cannot unilaterally move the prize.
- When a tournament is cancelled or expires without completion, sponsors withdraw refunds according to their respective actual contributions.
- All fund operations are signed by user wallets; the backend does not hold operational signing private keys.
- The original game engine and Agent decision code are reused. Without an LLM configuration, simulated runs are clearly displayed, without claiming to be real model output.

## Roles and Stakeholders

| Role | Business permissions | Constraints |
| --- | --- | --- |
| Platform administrator | Grants/revokes the on-chain organiser and auditor roles | Has no ability to withdraw the prize pool arbitrarily |
| Organiser | Creates tournaments, starts them, triggers matches, submits results, cancels where the rules allow | Can only manage their own tournaments |
| Participant | Creates Agents, registers/withdraws, views evidence, raises disputes, claims the prize after winning | Can only register and claim for their own wallet |
| Sponsor | Funds the prize pool, views funds and results, raises disputes/claims refunds when eligible | Cannot freely withdraw the prize pool once the tournament has started |
| Auditor | Views evidence, approves results, handles disputes | Must be identity-separated from the organiser, participants and sponsors of that tournament |

Participant and sponsor are business identities of a wallet; admin/organizer/auditor are on-chain roles that actually enforce access control. JWT only verifies backend identity; fund permissions are ultimately verified by the contracts. Hiding buttons in the frontend is not a security control.

## Assignment Requirement Mapping

| ID | Acceptance target | Acceptance evidence |
| --- | --- | --- |
| R01 | Concrete problem, users, existing process and blockchain suitability analysis | Report, sources, real interview/scenario test records |
| R02 | At least three differentiated role permissions | Permission matrix, unauthorised-access rejection tests, three-wallet demo |
| R03 | MetaMask signature login and on-chain transactions | Nonce anti-replay tests, recordings of user operations, transaction receipts |
| R04 | At least three genuinely interacting contracts | Contract diagram, deployment addresses, tests and ABI |
| R05 | At least ten categories of independent on-chain business transactions | CONTRACT.md list, success/failure tests for each transaction |
| R06 | REST API, persistent database, error handling | API.md, data model, restart/idempotency/failure cases |
| R07 | Responsive Dashboard, fund ledger, events and visualisation | Multi-role screenshots, desktop/mobile check records |
| R08 | Gas estimation, optimisation analysis, smart contract tests | Gas report, controlled experiments, Foundry output |
| R09 | Reproducible public cloud deployment | Public URL, health check, version/chain/deployment block information |
| R10 | Quantifiable security, performance, usability and business impact | Experimental method, raw data, statistical results and limitations |
| R11 | Complete delivery and traceable individual contributions | 10–15 page report, diagrams, repository, manual, slides, demo, contribution report |

## Security and Non-functional Requirements

1. Each tournament's funds are accounted for independently; cumulative prizes/refunds paid do not exceed the amount actually received.
2. No duplicate prize claims, duplicate refunds or duplicate bookkeeping through event replay.
3. Non-existent tournaments, zero-amount contributions, illegal state transitions and expired registrations are not accepted.
4. Settlement must go through result submission and a full dispute window; there is an exit path when completion is impossible by the deadline.
5. Login challenges are generated by the server, bound to the wallet, domain and chain ID, consumed once and have an expiry time.
6. Keys are not returned through the API; the course service does not accept arbitrary external LLM URLs or user model API keys.
7. Complete match actions, configuration, participants and result snapshots are saved, producing a recomputable evidence hash.
8. The indexer backfills from the deployment block and saves cursors and unique event keys; indexing can continue after a database restart.
9. When the chain is disconnected, unsynchronised/unavailable is shown, and local guesses are not displayed as completed payments.
10. User-cancelled signatures, contract reverts, pending transactions and RPC errors have clear feedback.
11. Normal deployment uses HTTPS, the minimum necessary configuration and persistent storage. On 2026-10-10 the user confirmed that the course delivery prioritises real local matches and demos, and allows the Render free demo environment to reset SQLite after sleep/restart/redeployment; this exception must be explained in the report and the video. It does not represent completed acceptance of cloud persistent storage, and evidence of actual matches and fund validation still needs to be saved locally.

## Evaluation Plan (Targets, Not Yet Results)

- Security: unauthorised access, replay, double claiming, cancellation/refund, fund conservation and wrong-chain cases pass.
- Reliability: tournaments/events can be recovered after a backend restart, and duplicate logs do not double-count amounts.
- Performance: record API p50/p95, indexing latency, match duration and gasUsed for each transaction.
- Usability: invite users in different roles to complete create/sponsor/register/review/claim tasks, recording success rate, time taken and points of confusion.
- Business impact: under the same review and dispute rules, compare the steps/time of manual reconciliation and system reconciliation; testnet block times must not be compared directly with industry end-to-end processing cycles.

## Out of Scope and Actual Limitations

This round does not extend to RAG, multi-chain, NFTs, knockout tournaments or real fiat on/off-ramps. The system does not automatically judge the authenticity of match results: the auditor, backend and LLM still have trust boundaries. Real adoption would also need assessment of identity verification, tax, privacy, asset rules, gas costs and the wallet usage barrier. The course allows reuse of the old project's rules, and each member's contributions must be stated truthfully.

## Why Blockchain, and When a Centralised Solution Is More Suitable

The key assumption of this design is that the organiser, sponsors, developers and auditors belong to different organisations and are unwilling to believe the prize pool has been secured merely on the basis of a screenshot of the organiser's database.

| Question | Conventional centralised platform | This project's on-chain mechanism | Remaining limitations |
| --- | --- | --- | --- |
| Are funds really pre-funded? | Relies on platform accounts, bank records or verification by a trusted third party | ERC20 escrow balances and each tournament's ledger can be queried independently | Test tokens have no real-world value; real tokens carry separate issuance and asset risks |
| Can the rules be changed ad hoc? | Relies on platform governance, permissions and audit procedures | Participant count, auditor, deadlines and distribution rules are fixed after creation | The administrator still controls roles; on-chain code defects may be irreversible |
| Who decides and executes payment? | Internal platform approval and payment accounts | Organiser submits, independent auditor approves, winner claims personally | Review still requires trusted people and evidence |
| How to exit if the platform becomes unreachable? | Relies on service recovery or external recourse | Expiry cancellation and refunds by original contribution require no server signature | Users still need a wallet, RPC and gas, and the rules cannot accommodate every offline dispute |
| How to verify history? | Platform logs and exported reports | Events can be replayed independently, and original evidence can be compared against on-chain hashes | Originals must be backed up; on-chain transparency does not mean personal identities are public |

If all participants already fully trust the same organisation, an ordinary database, bank escrow and audit logs may be cheaper and easier to use. The course report should explain that this project targets a scenario where multiple parties lack a commonly trusted operator, rather than claiming that centralised technology cannot make payments.

## Real Adoption and Ethics Discussion

- Registration prompts, match records and review reasons can be queried publicly; the user manual should remind users not to upload personal private information or model credentials. Only the necessary addresses, amounts and hashes are stored on-chain, to avoid permanently writing personally identifiable information on-chain.
- Different wallet addresses do not prove independent real-world identities. The organiser and auditor may still collude; the current mechanism mitigates unilateral action by a single wallet but does not solve collusion between real-world identities.
- Timeout refunds protect sponsor funds, but the time participants have already invested may not be compensated. When the organiser/auditor becomes unreachable, the current system cannot guarantee that the participant ultimately receives the prize.
- A single valid dispute enters a unified ruling state; multiple parallel cases, appeals, multi-signature juries or auditor replacement are not currently supported.
- Free registration and a sponsored prize pool reduce the project's dependence on spectator betting; a real launch would still require business and professional staff in the target region to assess competition, tax, asset and data-processing requirements. This course prototype does not provide legal or compliance conclusions.
- Before a public deployment, the wallet barrier, gas, RPC/LLM service costs, administrator key custody, the supply of independent auditors, database backups and availability need to be assessed.
