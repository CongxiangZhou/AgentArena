# Presentation notes and sources

## 01 Agent Arena

SC 6113 course project: financial infrastructure for small AI tournaments. The companion video follows tournament 13 on the local Anvil network with demonstration DUSD tokens. A local environment keeps the complete wallet and AI workflow responsive and easy to follow. Project repository: https://github.com/aokamutidao/agent-arena-escrow.

## 02 The prize settlement problem

FACEIT's official prize-claim guidance describes team prize breakdowns, account verification, a dispute waiting period and weekly payment batches. It provides evidence of settlement complexity, rather than a measured performance baseline for this prototype. The project focuses on small AI tournament organisers, sponsors, Agent developers and independent auditors. Source, accessed 10 October 2026: https://support.faceit.com/hc/en-us/articles/9718204637596-How-do-I-claim-a-cash-prize-such-as-a-League-or-Cash-Cup-prize . Project repository: https://github.com/aokamutidao/agent-arena-escrow.

## 03 Five stakeholders, distinct authority

The contract grants administrator, organiser and auditor roles. Participants and sponsors are ordinary wallets with tournament-specific business identities. A winner remains a participant. The designated auditor cannot also organise, participate in or sponsor the same tournament. The role matrix and contract implementation establish these permissions. Project repository: https://github.com/aokamutidao/agent-arena-escrow.

## 04 Tournament lifecycle

A tournament starts after registration closes, with two registered wallets and a funded prize. Result submission preserves the full review window. Cancellation or expiry enables each sponsor to withdraw their own remaining contribution. Every financial write requires a wallet signature. The submitted video performs the main prize workflow and displays existing refund history. Project repository: https://github.com/aokamutidao/agent-arena-escrow.

## 05 A funded prize before the match

Source: the supplied tournament-preparation recording, at 152 seconds. Tournament 13 shows two registered participants, 100 DUSD held in escrow and the sponsor's post-funding balance of 375 DUSD. ERC20 approval grants an allowance to PrizeEscrow, while the separate funding transaction transfers the tokens. DUSD is a demonstration token with six decimal places.

## 06 Real AI match, recorded round by round

Source: the supplied battle recording, including the match statistics near 120 seconds. Tournament 13 uses qwen3.5-plus and completes 15 rounds with 30 successful model calls and no failed calls. Blue wins with 10 HP. The call journal records 30,543 input and 513 output tokens. Its cumulative request latency of 66.96 seconds sums both players' calls and is not match wall-clock time. The saved evidence links Agent definitions, decisions, actions and final state.

## 07 Independent review, then the winner claims

Sources: the supplied audit recording at 63.5 seconds and prize-claim recording at 24.5 seconds. Approval leaves the 100 DUSD prize in escrow until the winning wallet claims it. Blue's wallet balance rises from 300 to 400 DUSD. This tournament's remaining escrow becomes zero. The evidence commitment is 0x00bc8fa3e7148c3dfbc98c0bb823c8a7448a37a1118f43ba741aa4a655fb1b22.

## 08 Full-stack architecture

The interface uses Next.js 14, TypeScript and wagmi. Go and Gin provide the REST API and match runner. SQLite stores profiles, rounds, evidence and query projections. MetaMask signs contract transactions. The backend reads confirmed chain events and does not hold a transaction-signing key. The architecture and API documentation describe this boundary. Project repository: https://github.com/aokamutidao/agent-arena-escrow.

## 09 Four interacting application contracts

TournamentManager coordinates TournamentRegistry, PrizeEscrow and ResultReview. CourseToken provides the separate DUSD demonstration asset. The interface defines 13 business operations: create, register, withdraw registration, fund, start, submit result, dispute, approve, resolve dispute, cancel, expire, claim prize and claim refund. ERC20 approval and administrative role changes are additional supporting transactions. Project repository: https://github.com/aokamutidao/agent-arena-escrow.

## 10 Security and evidence controls

Login uses a single-use nonce bound to the application domain, chain and contract deployment. Contracts enforce operational roles. Prize and refund claims use separate event accounting, SafeERC20, reentrancy protection and one-time claim state. Registration commits an immutable Agent hash. The canonical evidence hash detects changed record bytes, while the designated auditor evaluates result correctness. These controls and their failure cases appear in the contract and backend tests. Project repository: https://github.com/aokamutidao/agent-arena-escrow.

## 11 Validation and measurable results

The course evidence contains 41 Foundry contract tests and 42 frontend tests, with successful Go test, race and vet runs. Tournament 7 is a separate MetaMask acceptance run with nine confirmed receipts and 100 DUSD paid. Tournament 13 is the supplied demonstration. Committed gas snapshots for 1, 16 and 64 sponsors show constant claim costs of 66,162 gas for a prize and 63,976 gas for a refund. Controlled calls differ from standalone transaction receipts. Continuous integration: https://github.com/aokamutidao/agent-arena-escrow/actions/runs/38061976954. All four jobs passed for main revision 9e8e42981529ab0fd59f6ff3504f05cab7eb1389. Project repository: https://github.com/aokamutidao/agent-arena-escrow.

## 12 Submission and reproducibility

SC 6113 submission. Project repository: https://github.com/aokamutidao/agent-arena-escrow. Continuous integration: https://github.com/aokamutidao/agent-arena-escrow/actions/runs/38061976954. All four jobs passed for main revision 9e8e42981529ab0fd59f6ff3504f05cab7eb1389. Public application: https://agent-arena-course-web.onrender.com. API: https://agent-arena-course-api.onrender.com. The public application and API are live on Render Singapore. Six HTTPS checks passed on 10 October 2026: homepage, health, readiness, configuration, tournaments and dashboard. Browser checks confirmed the homepage, Chinese/English switching, Sepolia synchronization and the installed MetaMask connection entry. Cloud execution uses deterministic simulation. This smoke test did not sign a wallet request or execute financial transactions; the companion video verifies the full local workflow with real model calls. Sepolia chain 11155111, manager 0x4EAFee134156c65ae64887B5e7B861977C4CFa98, deployment block 11885235. Contract explorer: https://sepolia.etherscan.io/address/0x4EAFee134156c65ae64887B5e7B861977C4CFa98. The deployment checks cover contract code, module relationships and role configuration. The companion video provides the complete local business demonstration with English narration and bilingual subtitles.
