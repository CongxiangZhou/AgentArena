# From Code to Presentation: Course Learning Path

This document helps you explain the system you actually implemented. First run the [local demo](../DEPLOYMENT.md), then read in the order below; do not try to read the whole repository at once from the start.

## Step 1: Explain the Business Logic and Boundaries

Read `REQUIREMENTS.md` and `CONTRACT.md`, and draw the fund flow "sponsor 100 DUSD → escrow → review → winner claims", then draw the "tournament cancelled → original sponsor refunded" branch.

You should be able to answer:

- Who owns the assets? Who can change match results? Who can trigger payouts?
- Why does the sponsor fund the prize up front, rather than promising to pay after a win?
- Why is registration for the current two-player matches free, with no spectator betting?
- The blockchain can guarantee fund rules and that history cannot be arbitrarily altered; how does it depend on the auditor to judge off-chain facts?

Exercise: use three browser wallet identities to play the organiser, the participant and the auditor respectively, attempt an unauthorised operation, and record at which layer the rejection occurs.

## Step 2: Understand the Contract State Machine and Asset Safety

Read in order:

1. `contracts/src/CourseTypes.sol`: states and data structures.
2. `TournamentManager.sol`: external operations and role, time and state checks.
3. `TournamentRegistry.sol`: tournament state and registration snapshot.
4. `PrizeEscrow.sol`: per-tournament funding, total liabilities, prize claims and refunds.
5. `ResultReview.sol`: results, disputes, review evidence.
6. `contracts/test/`: use the tests to confirm the rules, especially the failure scenarios.

Write out the following invariants:

`Unpaid amount for a tournament = totalFunded - totalPaid`

`Total contract liabilities = sum of all prizes or refunds still claimable across all tournaments`

Explain the following design decisions:

- `onlyManager` protects the internal contracts; all wallets go through the same business entry point.
- ERC20 approval is granted to `PrizeEscrow`, which actually executes `transferFrom`.
- State changes first and funds are transferred afterwards; the reentrancy lock adds a further layer of protection.
- The claim pattern does not iterate over all sponsors; the gas for a single withdrawal does not grow linearly with the number of sponsors.
- Timeout refunds allow anyone to trigger cancellation, but each person can only claim the funds that belong to them.

Exercise: temporarily delete a state check, observe which test fails first, then restore the code. Write your own derivation of "why the old side-switching betting could overpay" and "why the new sponsorship ledger cannot".

## Step 3: Understand Backend Identity and Trusted Data

Read `backend/cmd/course-server/main.go`, then `backend/internal/tournament/`:

| File | Questions to understand |
| --- | --- |
| `model.go` | Why do the domain objects and the read-only ChainReader interface contain no signing/payment methods? |
| `auth.go` | What problem does each of the nonce, the signature and the JWT solve? Why must roles still be re-confirmed on-chain? |
| `store.go` | Which data can be rebuilt and which must be backed up? Why are amounts not stored as float? |
| `chain.go` | How does the ABI encode read-only calls and decode events? Why check the chain ID and contract addresses? |
| `indexer.go` | How is the block cursor persisted? Would reprocessing cause double-counting? How are deep reorgs handled? |
| `runner.go` | How are matches run, saved and resumed, and how is complete evidence generated? |

Exercise: after signing in once, replay the same nonce; restart the service; re-sync the same batch of events. Confirm, respectively, the rejection, the recovery, and that funds are not accumulated twice.

The `financial_ledger` in the database comes from confirmed events. A successful HTTP request, the frontend showing "Submitted", or the wallet popping up a signature prompt does not mean that funds have actually been transferred.

## Step 4: Understand the Agent's Capabilities and Limitations

Read the original `backend/internal/agent/agent.go`, `prompt.go`, `parser.go`, and the course `runner.go`.

The current Agent builds its input from the game state and its personality, calls the model, parses the action, validates the action, and then passes it to the deterministic engine for execution. Simulation mode uses a clearly labelled rule-based strategy, suited to stable demos and tests.

Evidence must include the model/simulation mode, the Agent definitions, the initial map, each round's actions and results, and the final winner. The hash can be used to verify that the downloaded raw JSON has not been altered; it does not prove that the backend did not fabricate the record before the first submission. Independent review therefore remains important.

Exercise: download the evidence and recompute the Keccak-256 hash; change the action in just one round and confirm that the hash changes. Explain why hashing only the final health values is not enough.

Rule retrieval, tool calling, memory, citation traceability and evaluation sets will be studied in the later RAG phase. In this round, do not put these not-yet-implemented capabilities on your CV.

## Step 5: Understand the Frontend Wallet Experience

Start with `frontend/src/lib/course.ts`, `useCourse.ts` and `useWalletTransaction.ts`, then read the home page and `app/tournaments/[id]/page.tsx`.

Trace a single sponsorship: enter a decimal amount → validate decimal places → convert to integer units → check the wallet network → estimate gas → approve → wait for the receipt → fund → wait for the business receipt → refresh on-chain and indexed state.

Exercise: in turn, reject a signature, switch wallets, switch networks, set an insufficient balance, and register after the deadline, observing whether the user receives understandable feedback. Verify that the old wallet's login session is not carried over to the new wallet.

## Step 6: Develop Your Own Account of the Project

The following is a writing framework, not unverified results:

> I refactored the original AI battle prototype into a tournament prize escrow system. The core work included.... I found ... problems, fixed them by ..., and proved the fixes with ... tests. The chain is responsible for ..., and the backend is responsible for .... The currently verified scale is ..., and the actual limitations are ....

Questions to prepare first:

- Why use four interacting contracts rather than one large contract?
- Why can a JWT not replace on-chain permissions?
- How is idempotency guaranteed from chain events to the database?
- What happens if the service restarts in the middle of a match?
- What happens if sponsored funds are never settled?
- How do you prove the ledger amounts are correct? How is precision handled?
- If the auditor is malicious, has their role revoked or becomes unreachable, what can the system still guarantee?
- How would you migrate from single-instance SQLite to multiple instances? Which consistency assumptions would need to change?

Fill in personal contributions based on the code, tests, documentation and verification records actually completed, distinguishing old hackathon work from new course work.
