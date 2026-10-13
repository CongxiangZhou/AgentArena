# Agent Arena User Manual

SC 6113 | Tournament prize escrow | 10 October 2026

SC 6113 | Group project

This manual explains how an organiser, sponsor, Agent developer and auditor complete a sponsored two-player AI tournament. DUSD is a demonstration token. Connecting a wallet, signing in and signing a blockchain transaction are three different actions.

## Start with the correct environment

Verified public access: https://agent-arena-course-web.onrender.com. The public instance uses Sepolia (11155111) and deterministic simulation. The recorded local environment below uses real model calls. See the deployment attachment for API and contract details.

For the recorded local environment, open `http://localhost:3002`. Use MetaMask in a browser with the extension installed. Local Anvil has chain ID 31337 and RPC `http://127.0.0.1:18545`. The Go API runs at `http://localhost:18081`. A matching chain ID alone is insufficient: check the RPC port and contract addresses in Network details. Local test accounts are for this isolated demonstration only.

| Role | Local demonstration wallet | Main responsibility |
| --- | --- | --- |
| Organiser | 0x7099…79C8 | Create, start and submit results |
| Auditor | 0x3C44…93BC | Inspect evidence and approve or resolve |
| Red developer | 0x90F7…b906 | Register its Agent |
| Blue developer | 0x15d3…6a65 | Register and claim if it wins |
| Sponsor | 0x9965…A4dc | Approve, fund and claim eligible refunds |

## Connect and sign in

1. Click Connect Wallet, select MetaMask and choose the required account.

2. Check the network and the address displayed at the top of the platform. Click Sign in and sign the unchanged login message. Login does not transfer tokens or require a gas fee.

3. Confirm the displayed role. Ordinary user means the wallet has no administrative role; it may still sponsor or register for an event. The event's Your next step panel shows its specific eligibility.

4. After switching account or network in MetaMask, sign in again. Do not assume the old account's permissions still apply. The header address and wallet popup should agree before each transaction.

Use the language button in the top bar to switch between English and Chinese. The selection persists in the current browser. User-created titles, prompts, raw evidence and wallet diagnostic text keep their original language.

<!-- PAGE -->
# 1 Create an event and register Agents

## Organiser creates the tournament

In Tournaments, expand Create tournament. Enter a clear title, the designated auditor address, an optional rules link, registration deadline, final settlement deadline and review period. Use enough registration time to switch accounts and fund the event. The normal default is 15 minutes for registration, 24 hours for final settlement and 60 seconds for review.

The auditor must be different from the organiser and entrants, and cannot sponsor the event being audited. The organiser must hold its contract role. Check deadlines against the platform's current chain time, then choose Preview and create tournament. Review the transaction preview and confirm in MetaMask. Wait for a successful receipt and open the new event.

## Each developer prepares an Agent

Switch to the developer wallet and sign in. Open My Agents, enter a name and strategy or personality, and save the definition. Saving an Agent is an authenticated backend operation and does not require a blockchain transaction. Avoid putting personal information, API keys or other secrets in a prompt; definitions and evidence may be inspected by others.

An Agent is immutable after saving. To change strategy, create another definition. The definition fingerprint identifies its exact content and owner. The model endpoint is configured by the platform; developers do not upload individual model credentials.

## Register red and blue

Choose Enter a tournament from the saved Agent or open an event that is still accepting registrations. Select Register, choose the Agent owned by this wallet, and request the transaction preview. Confirm in MetaMask. Successful registration adds the wallet and Agent fingerprint to the event.

Repeat with the second developer wallet. The first registered wallet is red and the second is blue. Exactly two registered participants are required. Neither two connected browsers nor two saved profiles replaces two successful registration transactions.

Before the deadline, a registered developer may withdraw and register another definition. After it closes, registration, withdrawal and sponsorship are no longer allowed. Two filled slots do not let the organiser start before the deadline.

## The standard transaction sequence

For every on-chain action, click the business button, inspect the preview, then click Confirm and sign in wallet. Confirm the wallet popup and wait for Transaction confirmed. ETH network fees are separate from DUSD business amounts. If a transaction has already been submitted, inspect its receipt instead of submitting the same action repeatedly.

<!-- PAGE -->
# 2 Sponsor and run the match

## Sponsor funds the prize

Sign in with a sponsor account, open the event before registration closes, and choose Sponsor prize. Enter the exact DUSD amount, for example 100. Amounts can have up to six decimal places.

The first step is token approval. The spender must be the configured PrizeEscrow contract. Approving 100 DUSD permits that amount to be transferred; it does not yet increase the prize pool. Once approval is confirmed, click Confirm sponsorship and approve the second wallet transaction. Verify the prize pool increased by 100 DUSD and the sponsor's available balance decreased by the same amount.

The designated auditor cannot sponsor this event. A sponsor cannot withdraw a running event's contribution at will. Refunds become available only through the cancellation rules.

## Organiser starts the competition

Switch back to the event organiser and sign in. Wait for the platform's chain time to reach the registration deadline. Check that both Agent registrations are present and the funded prize is positive. Use Start tournament and confirm the wallet transaction. A successful start changes the event to its active state.

Click Run Agent match. This starts a backend job; it is separate from the start transaction and does not itself submit a result or transfer funds. The live match shows the board, HP, actions and model-call counts. A real-model run is labelled accordingly. A deterministic simulation has an explicit simulation label.

## Replay and inspect evidence

After the match completes, use Play replay, pause, the round slider and previous or next controls to inspect the game. Expand round actions when needed. Open or download the complete evidence and wait for the content-hash verification indicator.

The recorded event 13 completes 15 rounds and 30 successful model calls. Blue wins with 10 remaining HP. These numbers describe that recording, not every future run. Do not choose a winner from a label alone; inspect the final state and evidence.

If a model request or action parse fails, the job stops with a visible error. The platform may offer Resume when the organiser, event status and remaining budget allow it. A completed match cannot be overwritten by running it again. A draw has no awardable winner and follows the applicable cancellation route.

<!-- PAGE -->
# 3 Submit review and claim the prize

## Organiser submits the result

With the complete evidence viewed and verified, choose Submit result. Check the winner, tournament, evidence hash, signer and network in the transaction preview. Confirm in MetaMask. Successful submission starts the fixed review window; it does not pay the winner.

During the review window, a registered participant or actual sponsor can open a dispute. Enter the reason, save its evidence and sign the dispute transaction. A saved note alone is only a draft. It becomes a submitted dispute when the corresponding hash is recorded on chain.

## Auditor checks and decides

Switch to the designated auditor wallet and sign in. Open the event, inspect the replay, final result and complete evidence. Confirm that the displayed content matches the submitted chain hash. Wait until the full review window ends; the organiser cannot shorten it.

For an undisputed result, choose Approve result and confirm the transaction. For a disputed result, inspect the challenge, select a valid entrant as winner or cancel for refund, record a reason and sign Resolve dispute. Approval must occur before the final settlement deadline. Hash verification confirms content consistency; the auditor still judges whether the record supports the stated outcome.

## Winner receives the funds

Switch to the confirmed winner and sign in. The next action should display the claimable prize, such as Claim 100 DUSD. Review and confirm the transaction. Verify all three outcomes: a successful claim receipt, the wallet's token balance increasing by the prize, and the event's outstanding escrow becoming zero.

In the submitted recording, the blue balance changes from 300 to 400 DUSD. Other tournaments may still hold funds, so the global dashboard total need not be zero. A finalised event with a claim button is approved but unpaid; a claimed event has completed the payment step. The same prize cannot be claimed twice.

## Inspect the audit trail

Open Audit and select the tournament. Review funding, registrations, start, submission, approval and prize claim in order. Each entry includes the actor, transaction identity and chain location. Export current page JSON to retain that page's raw event records. Filtering and exporting the current page does not export the entire database.

<!-- PAGE -->
# 4 Refunds and troubleshooting

## Cancel and refund

Before play, the organiser may cancel with a reason. An underfilled event can expire after registration closes. An unfinished event can expire after the final settlement deadline. An eligible timeout can be triggered by any wallet. The designated auditor can also decide cancellation when resolving a dispute.

Cancellation enables refunds, but it does not automatically send them. Each sponsor must reconnect the original funding wallet, open the cancelled event and claim its displayed refundable contribution. Verify the refund receipt and balance increase. The recorded final video shows a historical refund page for explanation and does not execute another refund transaction.

| Symptom | Check | Next action |
| --- | --- | --- |
| Correct wallet but wrong role | Header address and contract role | Sign in again; ask the administrator to check role assignment |
| No available action | Event state, deadline, wallet and chain time | Follow Your next step and the displayed eligibility reason |
| Approval succeeded but pool unchanged | Whether the funding transaction was sent | Complete the separate sponsorship step |
| Pending or unknown transaction | Transaction hash and receipt | Use the progress/receipt view; avoid duplicate submissions |
| Wallet rejected signature | Whether anything was broadcast | Retry only when ready; rejection alone does not transfer funds |
| Receipt succeeded but dashboard lags | Indexed block versus chain head | Refresh after confirmation and index progress |
| Evidence mismatch or unavailable | Hash, network and original record availability | Stop approval and restore or inspect the correct evidence |
| Local account has no gas | Wallet network and local test funding | Use the correct local setup and demonstration account |

## Data and service operation

The source repository contains setup instructions and deployment configuration. Start the course server, not the historical hackathon server. Check `/health`, then `/ready`; a live process is not necessarily ready to index the financial deployment. The local chain and SQLite evidence should be backed up together when preserving the demonstrated environment.

The free cloud presentation instance may need time to wake, and its transient SQLite data may reset. Chain events can be indexed again, but original Agent definitions and match evidence require retained copies. Use the submitted video and evidence package to review the completed local run. Never import public development-account keys into a wallet used for real assets.
