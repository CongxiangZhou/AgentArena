# First Hands-On Experience: From Watching a Replay to Settling a Tournament Yourself

The top toolbar provides an **English / Chinese** toggle, and your choice is saved in this browser. The tutorial below uses the English-mode button names; for the mapping between the English and Chinese navigation, see the [user guide](../USER_GUIDE.md#switching-between-chinese-and-english). Switching language does not translate existing tournament names or raw evidence.

Applicable version: course version of Agent Arena. Verification date: 2026-10-10 (Asia/Singapore).

This guide is for you to operate yourself: first browse a real match and a completed refund, then create and complete a new tournament. Everything uses the local Anvil test chain and the DUSD test token, so there is no need to top up or buy ETH; participants do not need a personal model API key, as the platform has a real LLM configured. The current real example is #5 (`qwen3.5-plus`); the historical #1 is still `deterministic_simulation`, and the replay and evidence distinguish the actual mode.

The current environment and balances follow the CLI acceptance record in the [033 test report](../TEST_REPORT.md#2026-10-10-real-model-and-local-funds-loop-033); the [read-only snapshot](evidence/experience-preflight-2026-10-10.json) of the early simulation environment is kept for historical reference. 033 was signed by scripts using public Anvil test accounts and cannot be treated as MetaMask UI acceptance. The later MetaMask acceptance round, 034, has since passed with real wallet signatures (see the [034 test record](../TEST_REPORT.md#2026-10-10-metamask-main-tournament-acceptance-034-final-scope-passed)); what follows is an operating tutorial.

## 0. First, understand the current environment

| Item | Current value |
| --- | --- |
| Browser used | Chrome on this Mac, with the installed MetaMask extension |
| Frontend | <http://localhost:3002> |
| Backend health check | <http://localhost:18081/ready>, which should return `ready: true` |
| Local RPC | `http://127.0.0.1:18545` |
| Chain ID | `31337` |
| Network fee currency | Local test `ETH` |
| Prize token | `DUSD`, 6 decimals |
| Current Manager | `0xe7f1725e7734ce288f8367e1bb143e90bb3f0512` |

These addresses correspond to the 033 real-model environment; 3001 / 18080 keep the old demo and are not the entry point for new matches this time. Once you have opened the frontend and confirmed that the network status is normal, you can start; there is no need to start things again. If the services stop later, see the recovery instructions at the end of this document.

**The RPC port must also be checked.** Another old node on this machine may also use `31337`, so seeing the same chain ID does not mean you are connected to the right environment. This demo must use **18545**, not **8545**. The frontend entry point is `localhost:3002`, and the domain in the login message must match it.

In an embedded browser that does not support extensions, clicking MetaMask may show "Scan with MetaMask". For the local experience, use the wallet extension in desktop Chrome: on a phone, `127.0.0.1` refers to the phone itself and cannot reach this Mac's test chain directly.

## 1. Without connecting a wallet, spend a few minutes understanding the platform

Open the following pages in turn. No login or signature is needed at this stage.

| Page | What you can explore | State at the time of this check |
| --- | --- | --- |
| [Overview](http://localhost:3002/) | Sponsorship, prizes paid, refunds, funds pending settlement and indexing progress | Post-033 snapshot: total sponsorship 450, paid 200, refunded 250, pending settlement 0 DUSD |
| [Tournaments](http://localhost:3002/tournaments) | Tournament status, prize pool, number of participants | 6 tournaments in total after 033 |
| [Real LLM tournament #5](http://localhost:3002/tournaments/5) | Map, HP, each model call, full evidence, settlement events | 14 rounds, 28 successful calls; Blue claimed 100 DUSD |
| [Cancellation refund example #6](http://localhost:3002/tournaments/6) | Cancellation reason, refund events and balances | 25 DUSD refunded; no model was run for this tournament |
| [Historical simulated tournament #1](http://localhost:3002/tournaments/1) | Deterministic simulation replay and settlement events | Blue won, 100 DUSD claimed; no LLM was called |
| [Historical dispute example #2](http://localhost:3002/tournaments/2) | Dispute/decision explanation, cancellation and refund events | 25 DUSD refunded; uses clearly labelled test result material, not a real model match |
| [Audit](http://localhost:3002/audit) | Select a tournament, look up events, paginate, export the current page as JSON | Lets you check records of creation, sponsorship, registration, review, claims and so on |

On the #5 page, find "Match replay", click "Play replay" or drag the round slider, and watch the positions and HP change; "Follow latest round" returns to the last round. Expand "View round actions and decision records" to see detailed actions and check the successes, failures, durations and usage of the real calls, then go to "Match evidence" and click "Show full evidence" or "Download evidence JSON". This tournament should show "Content matches the on-chain record", and "Financial and activity records" below lets you check the settlement events.

#1 and #5 are already complete and cannot be rerun or have their prizes claimed again.

**The old #3 has been cancelled and its 200 DUSD refunded, and #4 has also been cancelled (no sponsorship).** In the early snapshot #3 once showed "Expired · Refund pending"; that state is no longer the current starting point. A refunded tournament cannot be claimed again; to practise cancellation and refund, create a separate new tournament, as described in Section 6.1.

The amounts above are a snapshot taken before this session's operations; it is normal for them to change after you operate.

## 2. One-time MetaMask setup

### 2.1 Finish wallet initialisation

If you are still on the "Create password" or backup page, complete it yourself first. Do not reset an existing wallet, and you do not need to export your own personal wallet's private key.

A file stored only locally and ignored by Git has been prepared for this session:

**[Open the public demo accounts file](../../.local/course-demo-wallets.txt)**

Local file: `.local/course-demo-wallets.txt` (generated by the local preparation script and not distributed with the repository).

The private keys in the file come from the Anvil test mnemonic that is already public in the code and contain none of your personal wallet secrets. Anyone can derive these accounts, so **use them only for local testing and never deposit real assets to these addresses**. They are not newly created personal wallets.

### 2.2 Import the demo accounts

In MetaMask's account selector, choose "Add wallet" → "Import an account" → the private key option. Copy the "public test private key" for the corresponding identity from the file, and after importing, name it after the role. Menu wording may change between versions; see the [official import instructions](https://support.metamask.io/start/use-an-existing-wallet/).

The first complete walkthrough needs the last five accounts; you do not need to import the administrator for now. Verify by address rather than relying on the Account numbers MetaMask generates automatically.

| Identity / suggested name | Address | Purpose and balance at the time of this check |
| --- | --- | --- |
| Administrator (optional) | `0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266` | Already has administrator rights; can manage organiser and reviewer roles |
| Organiser | `0x70997970C51812dc3A010C7d01b50e0d17dc79C8` | Already has organiser rights; creates, starts, runs and submits results |
| Reviewer | `0x3C44CdDdB6a900fa2b585dd299e03d12FA4293BC` | Already has reviewer rights; approves results or resolves disputes |
| Red participant | `0x90F79bf6EB2c4f870365E785982E1f101E93b906` | Creates their own Agent, registers and claims the prize after winning |
| Blue participant | `0x15d34AAf54267DB7D7c367839AAf71A00a2C6A65` | As above; balance 200 DUSD after 033, from the prizes of #1 and #5 |
| Sponsor | `0x9965507D1a55bcC2695C58ba16FB37d819B0A4dc` | Balance 800 DUSD after 033, can sponsor directly; check again before new operations |

Each of these accounts has about 10,000 local test ETH, enough to pay the demo gas. The organiser and participants do not need DUSD to create a tournament or register.

If the local accounts file does not exist, you can obtain the **organiser's public test private key** with the command below in a terminal with Foundry installed; replace the final index with 2, 3, 4 or 5 to get the reviewer, Red, Blue or sponsor respectively. This command does not read your real wallet.

```bash
cast wallet private-key 'test test test test test test test test test test test junk' 1
```

### 2.3 Add the local network

In MetaMask's network selector, add a custom network and fill in:

| Field | Value |
| --- | --- |
| Network name | `Agent Arena Local` (display name only) |
| RPC URL | `http://127.0.0.1:18545` |
| Chain ID | `31337` |
| Currency symbol | `ETH` |
| Block explorer URL | Leave blank |

If a network with Chain ID 31337 already exists, check it and select the configuration whose RPC is 18545; do not judge only by an old network name. After saving, switch to this network. See the [official instructions for adding a network](https://support.metamask.io/networks-and-sidechains/managing-networks/how-to-add-a-custom-network-rpc/).

### 2.4 Connecting and logging in are two steps

1. Open <http://localhost:3002/tournaments> in Chrome.
2. Click "Connect wallet" at the top, choose MetaMask in the (localised) wallet dialog, then in the extension select the demo account you want to use and connect.
3. Confirm that the address at the top of the page matches this account and that the network is the local chain.
4. Click "Sign in" on the page, check that `localhost:3002`, Chain ID 31337 and the Manager in the message match the current environment, then sign in MetaMask.
5. The organiser should show "Organizer", the reviewer "Auditor" and the administrator "Admin". Ordinary participant/sponsor wallets show "Standard user" at the top: this means they have no global management role, not that they have registered. On the details page, "Your next step" and "My tournament account" separately mark identities for that tournament, such as participant or sponsor.

**Every time you switch accounts: switch in MetaMask → check the address on the page → click "Sign in" again.** If the page address does not follow the change, check which accounts are connected to this site in the wallet, or disconnect and reconnect on the page. Do not just change the account nickname.

Logging in is an identity signature; it does not transfer funds or spend gas. Later operations such as creating a tournament, registering and sponsoring are on-chain transactions.

### 2.5 Remember the common steps for every transaction

All on-chain business buttons follow this sequence:

**Click the specific business button → a transaction confirmation window pops up automatically, checking and estimating first → check the operation, amount and estimated network fee → click "Confirm and sign in wallet" → confirm in MetaMask → wait for "Transaction confirmed", then click "Done".**

For example, "Register and review fees", "Start match" and "Claim 100 DUSD prize" only prepare the transaction on the first click and do not send it automatically; there is no need to scroll looking for a confirmation card at the bottom of the page. The ETH in the pop-up is the network fee, to be checked separately from the DUSD amount in the operation. After changing a form, you must click the business button again and re-estimate.

You can close the pop-up with Escape or "Hide window". Closing it does not cancel a transaction that has already been sent; the bottom right of the page (the bottom on mobile) keeps "View transaction progress" or "View and check receipt", which can reopen it. While awaiting a signature, on-chain confirmation, or when the final state is unclear, do not submit again; when the state is unclear, click "Check transaction receipt".

After success, wait for the data to refresh, usually about 10 seconds. On the details page, "Prize and rules" shows the registration, review and final deadlines, as well as the "Current chain time"; do not rely only on your computer clock. The network status at the top can be expanded to "Network details" to view the contracts and synchronisation progress.

## 3. First, look at a completed cancellation refund

This step reads real records first and needs no new signature. The cancellation transaction and claiming the refund are two independent operations.

1. Open [tournament #6](http://localhost:3002/tournaments/6) and look at the "Cancelled" status, the cancellation reason and the fund records.
2. In the activity log, check the 25 DUSD sponsorship, the cancellation event and `SponsorRefunded`; you cannot conclude that a refund was made from "Cancelled" alone.
3. In the 033 CLI acceptance, the sponsor actually claimed 25 DUSD, and the balance rose from 775 to 800; the corresponding receipts and balance records are in the test report. This is not a new MetaMask operation in this round.
4. If you want to reproduce it yourself, follow Section 6.1 to create a new 25 DUSD tournament, cancel it before it starts, and then claim the refund from the original sponsor wallet.

Both #3 and #6 have already been refunded and cannot be claimed again.

## 4. Prepare two participant Agents before creating a new tournament

Preparing the accounts and Agents first avoids being busy with setup after the tournament is created and missing the registration window.

1. Switch to the **Red participant**, sign in, and open [My Agents](http://localhost:3002/agents).
2. If `Aggressive Scholar` already exists, you can use it directly; alternatively, in "Create an agent to compete" choose "Aggressive", enter `My Aggressive Agent` as the "Agent name", check that "Strategy and personality" is `aggressive`, and click "Save agent and prepare to compete".
3. Switch to the **Blue participant** and sign in again, then use the existing `Patient Guardian`, or choose "Defensive" and create `My Defensive Agent` with the strategy `defensive`.
4. Both sides should see their own card under "Saved agents"; "View definition fingerprint" expands the content hash, and "Compete with this agent" goes to the tournaments page. Saving a definition writes only to the backend and does not pop up an on-chain transaction confirmation.

New matches in the current 3002 environment are played by a real LLM reading the Agent's strategy; you can also reuse the existing `Local LLM Red` / `Local LLM Blue` definitions in this wallet. The historical simulator only recognises keywords such as `aggressive` and `defensive`, which does not represent real model performance. A definition cannot be modified once saved; to adjust the strategy you need to create a new definition.

## 5. Complete a new tournament yourself

### 5.1 The organiser creates it

Switch to the **organiser** and sign in, open [Tournaments](http://localhost:3002/tournaments), click "Create tournament" to expand the form, and fill in:

| Field | Suggested value for a first walkthrough |
| --- | --- |
| Tournament name | `My First Full Walkthrough` |
| Designated auditor wallet | `0x3C44CdDdB6a900fa2b585dd299e03d12FA4293BC` |
| Rules link (optional) | Leave blank |
| Registration closes in (minutes) | `15` (the default for a new form), leaving time for first-time account switching, registration and sponsorship |
| Final settlement deadline in (hours) | `24` |
| Result review period (seconds) | `60` |

Click "Preview and create tournament", check the operation, designated reviewer and registration deadline in the automatic pop-up, then send it using the common transaction steps. After success the new tournament appears in the list; note its number and always go into this tournament from now on; do not assume it will be #4. "Close creation form" only collapses the input area and does not undo a creation transaction that has been sent.

Deadlines are calculated from **the chain time when you click "Preview and create tournament"**. Do not leave it for a long time after previewing before sending. If you want to shorten the wait, you can create the next tournament with a shorter registration window once you are familiar; an existing tournament's deadlines cannot be changed.

### 5.2 The sponsor deposits 100 DUSD

Switch to the **sponsor**, sign in, and go to the new tournament's details:

1. In "Participate in this tournament" choose "Sponsor prize", and enter `100` for "Sponsorship amount (DUSD)", or click the `100 DUSD` quick-select button.
2. Click "1. Approve 100 DUSD", check and sign in the automatic pop-up, and wait for confirmation. The approval target should be this environment's prize escrow contract `0x9f1ac54bef0dd2f6f3462ea0fa94fc62300d3a8e`.
3. Once the page shows the amount as approved, click "2. Confirm sponsorship", check **100 DUSD** in the new pop-up and sign to confirm again.
4. Signs of success: "Total tournament prize" is **100 DUSD**, "My sponsorship · Held in escrow" is **100 DUSD**, and the sponsor's balance has decreased by 100.

Approval only allows the contract to deduct tokens according to the rules; it does not increase the prize pool. Even if there is already sufficient approval, you can still go through the two steps above in full; do not mistake a successful approval for a successful sponsorship. The designated reviewer cannot sponsor a tournament they review.

### 5.3 The two participants register separately

First switch to the **Red participant** and sign in, choose "Compete" in the new tournament's "Participate in this tournament", select your Agent, click "Register and review fees", then send and confirm as prompted by the pop-up. You should see **1 / 2** registered.

Then switch to the **Blue participant** and repeat: sign in, select your Agent, register. You should see **2 / 2**. Whoever registers first is Red and whoever registers second is Blue, so follow the order above this time.

Registration is free, but the registration transaction consumes local test ETH. The organiser and the designated reviewer cannot be participants in their own tournament.

### 5.4 The organiser starts and runs the match

Switch back to the **organiser** and sign in, and wait until the details page's "chain time" reaches "Registration deadline". Even with two participants you still have to wait for the deadline; you can use this time to look at the funds and registration events.

1. In "Your next step" click "Start match", send it and wait for success. The on-chain status then becomes "Match in progress".
2. Click "Run Agent match". This is a backend job and does not require signing another on-chain transaction.
3. Wait for the page to automatically update the round records and model calls; the time taken depends on the model. Merely having an LLM configured does not mean the calls succeed. If a request or parsing fails, the match is interrupted; check the error first, and only click "Resume match execution" when the backend allows resumption.
4. Check that this tournament shows the real LLM mode and successful call records; when complete, view both sides' actions, positions, HP and the actual result round by round. You can play, pause, step to the previous/next round or drag the slider. If it shows deterministic simulation, it must not be recorded as real LLM acceptance.

Do not treat "starting the match" and "running the match" as a single step. The same completed match cannot be rerun to overwrite its evidence.

### 5.5 The organiser submits the result

1. Click "View match evidence" in "Your next step", or find "Match evidence", click "Show full evidence" or "Download evidence JSON", and actually check the content.
2. Check "Content verification passed"; once viewed it will show "Viewed". "Evidence hash and verification" can be expanded to show the full hash.
3. Return to "Your next step", click "Submit match result", and confirm and sign in the pop-up.
4. After success it enters "Review window open", and the page shows the nominated winner and the review deadline; the evidence area should show "Content matches the on-chain record".

**You must view or download the evidence first before the submit button is enabled.** Submitting the winner's address and evidence does not pay out automatically.

Go by the result actually shown for this tournament; do not assume Red or Blue will win. If it is a draw, the current rules have no winner and a winner cannot be submitted; you should wait for the final settlement deadline and then cancel for refunds. This limitation is also part of the current platform.

### 5.6 The reviewer confirms

Switch to the **reviewer** and sign in, go into the same tournament, view the replay and again expand or download the full evidence.

Wait until the chain time reaches the review deadline (60 seconds from the result submission for this tournament), click "Approve result and confirm winner" in "Your next step", then send and confirm.

Sign of success: the tournament status is "Settled · Ready to claim", showing the winner's address and a prompt that the prize is awaiting claim. The reviewer must be the one designated at creation; other wallets holding the "Auditor" management role cannot substitute.

### 5.7 The winner claims the prize

Check this tournament's winner address, switch to the corresponding **Red or Blue participant**, connect and sign in:

1. In "Your next step" click "Claim 100 DUSD prize" and send it using the common transaction steps.
2. After success the status shows "Prize claimed", the page indicates the prize has been claimed, the claim button disappears, and the winner's wallet balance increases by 100 DUSD.
3. Blue already has earlier prizes, so compare the **increase** rather than requiring every winner's final balance to equal 100.
4. In [Audit](http://localhost:3002/audit), select the new tournament and check `TournamentFunded`, `ParticipantRegistered`, `TournamentStarted`, `ResultSubmitted`, `ResultApproved` and `PrizeClaimed`.

If you start from the post-033 snapshot and complete only this section's 100 DUSD tournament with no other operations, the overview should show: total sponsorship **550**, prizes paid **300**, sponsorship refunded **250**, pending settlement **0 DUSD**, and the sponsor wallet balance **700 DUSD**. Actual acceptance still compares this tournament's balance increases and receipts, without presupposing the winner.

At this point you have experienced one complete path: **funds deposited in advance → registration → match → evidence → independent review → user withdraws by themselves**.

## 6. Then try cancellation, disputes and administrator rights (optional)

### 6.1 The organiser cancels proactively

Create another new tournament and have the sponsor deposit 25 DUSD. **Before it starts**, the organiser expands "Need to cancel this tournament?", enters "This is a cancellation and refund walkthrough" in "Reason for cancellation", clicks "Cancel tournament and review fees", and sends the transaction as prompted by the pop-up. There is no need to gather two participants first.

After the status changes to "Cancelled", switch to the sponsor to claim the 25 DUSD refund. The cancellation note is first saved in the backend; only once the cancellation transaction is confirmed does the on-chain state formally change.

### 6.2 Result disputes and resolution

This path needs another complete match. When creating it, we suggest setting the review window to **300 seconds** to leave time for switching accounts and writing explanations. After completing result submission through the normal flow:

1. Before the review deadline, switch to a participant or actual sponsor of that tournament, sign in and write a test dispute explanation, for example "Drill: requesting the auditor to re-check the result in the replay".
2. Expand "Disagree with the result? Submit a dispute", fill in "Grounds for dispute", click "Submit dispute and review fees", and send the transaction as prompted by the pop-up. Confirm that the status changes to "Disputed"; merely saving a draft explanation does not mean an on-chain dispute has been raised.
3. Switch back to the designated reviewer, sign in, view the evidence and explanation, and wait for the original review window to end. If you intend to rule a winner, you must view and verify consistent evidence; if the original evidence is missing or unreliable, you can choose to reject and cancel for refunds, still filling in a reason and waiting for the window to end.
4. Click "Review dispute and decide", and in "Review and resolve the dispute" choose the "Dispute decision": a winner, or "Reject result: cancel tournament and enable refunds". Fill in "Reason for decision", click "Submit decision and review fees", and send the transaction as prompted by the pop-up.
5. If you choose a winner, the winner claims the prize; if you choose cancellation, each sponsor claims their own refund.

Do not insert a dispute in the middle of your first normal walkthrough; otherwise you enter the resolution branch and can no longer click the ordinary approve button.

### 6.3 Withdrawing a registration and the administrator role

- Before the registration deadline, a registered participant can click "Withdraw from this tournament" in "Participate in this tournament"; after completing the transaction as prompted by the pop-up, the slot is freed; afterwards they can register again.
- Import the administrator account and sign in, then on the Tournaments page expand "Administrator: manage organizer and auditor roles", choose "Organizer" or "Auditor", enter the target wallet, click "Preview role grant" or "Preview role revocation", then sign as prompted by the pop-up. The roles for the first main flow are already configured and need no adjustment; if you want to practise, use another dedicated test wallet to avoid revoking the organiser/reviewer roles you are currently using.

## 7. If you get stuck, check by symptom

| Symptom | Check first |
| --- | --- |
| A QR code appears after clicking MetaMask | Use desktop Chrome + the MetaMask extension on this machine instead; the local chain does not use the mobile scanning path |
| The wallet shows 0 ETH, or the network looks right but transactions fail | Check the demo address and RPC **18545**; the same Chain ID may still connect to the old 8545 node |
| The wallet is connected but there is no create tournament button | Select the organiser `0x7099…79C8`, then "Sign in"; a newly created personal wallet has no organiser rights |
| Switched wallet, but the page still shows the previous address | Check the accounts connected to the site; if necessary disconnect and reconnect, and sign in again |
| My Agents is empty | Definitions belong to a wallet; confirm the currently signed-in address, or create a definition of your own |
| DUSD is not visible in the wallet | MetaMask does not necessarily display custom tokens automatically; first look at "Available wallet balance" under "My tournament account" on the details page. You can import the token manually using the contract address below |
| After clicking a business button, the tournament/balance did not change | You still need to click "Confirm and sign in wallet" in the automatic pop-up and confirm in MetaMask; clicking the business button only prepares the transaction |
| Approval succeeded but the prize pool is still 0 | You still need to click "2. Confirm sponsorship" and sign to send the sponsorship transaction |
| Two people are registered, but the start button is still disabled | Wait for the chain time to reach the registration deadline; also check the organiser identity and login |
| "Start match" is disabled or errors | The prize pool must be non-zero, two people registered, the chain time past the registration deadline, and a full review window must still fit; the old #3 has been cancelled, so create a new tournament |
| The match has run but the result cannot be submitted | First expand/download the evidence and confirm the hash matches and it is not a draw; the account must be the organiser |
| The review button is greyed out | Requires the designated reviewer, evidence viewed, matching hash, the original review window ended, and the final deadline not passed |
| The tournament is settled but nothing has arrived | You still need to switch to the winner's wallet to claim the prize; review does not transfer funds automatically |
| Nothing arrived after cancellation | You still need to switch to the sponsor wallet to claim the refund; cancellation does not transfer funds automatically |
| Prompted about nonce, expired login or 401 | "Sign in" again; if it is a wallet transaction nonce, first check the network and pending transactions, and do not send again |
| "Check transaction status" | Click "Check transaction receipt" rather than resending immediately; after hiding the pop-up, reopen it from the progress entry at the bottom |
| Events or balances on the page update a little late | Wait about 10 seconds for a refresh; check whether the indexed block on the funds page has caught up with the chain head |
| "Current chain time" keeps loading | Check that the RPC is reachable, that all three services are still running, and that the page and wallet configuration use the same node |

Current DUSD contract: `0x5FbDB2315678afecb367f032d93F642f64180aa3`, symbol `DUSD`, decimals `6`. This address describes only this deployment; after redeployment, go by <http://localhost:18081/api/config>.

## 8. How to resume the current environment after the services stop

The current API 18081 uses `.local/llm-demo/course.db` and shares the existing 18545 chain with the old API. Check ports and processes first; do not blindly kill processes or start things again.

If only the new API / frontend has stopped, follow the [local real-model guide](LOCAL_LLM_DEMO.md) and [running and deployment](../DEPLOYMENT.md) to restore the original configuration, database and the 3002 / 18081 services, and check `/ready`, the deployment addresses and the original tournaments. `./start.sh` does not automatically start Anvil or deploy contracts.

**Do not treat `smoke-course.mjs --serve` as the recovery command for the current environment.** It creates a new test chain and a temporary database and cannot continue the #5 / #6 records. If Anvil stops or the chain state is lost, first preserve the data and manually check it following the backup and recovery process; the hashes in the database cannot restore the full chain state, and the backup restore drill has still not been accepted.

## 9. Things you should be able to explain after the walkthrough

- What connecting a wallet, signing in and sending a transaction each do.
- Why sponsorship has two steps, approval and deposit, and why the winner must still claim the prize themselves after review approval.
- Which operations the organiser, reviewer, participants and sponsor can each perform.
- Where the replay and raw evidence are stored, and what the on-chain hash can verify.
- How expiry, proactive cancellation and dispute rejection lead to refunds.
- How the historical simulation of #1 differs from the real LLM record of #5; which checks the 033 CLI passed, what the 034 MetaMask acceptance and the public deployment added, and why external evaluation remains outside the completed scope.
