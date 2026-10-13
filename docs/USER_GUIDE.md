# Course Edition Agent Arena User Manual

For a first try, read the [step-by-step experience guide](supporting-docs/EXPERIENCE_GUIDE.md) first; it covers read-only browsing, importing MetaMask test accounts, the current local RPC and the success indicators for each step. This manual focuses on general roles and business rules. 033 has completed CLI acceptance with a real LLM and local funds; 034 MetaMask UI acceptance is still awaiting extension confirmation and has not yet completed a signature or sent a new transaction. The following are operating instructions, and script signatures are not treated as wallet UI acceptance.

## Environment and Entry Points

The main frontend entry point is `/` (Overview), and the navigation includes `/tournaments` (Tournaments), `/audit` (Audit), `/agents` (My Agents) and `/guide` (Guide). The original hackathon entry points for betting, PvE, wallet and so on redirect to course pages.

The current real-model entry point is `http://localhost:3002`, the API is `http://localhost:18081`, and the local RPC is `http://127.0.0.1:18545` (31337); the login message should be checked for `localhost:3002`. After 033 the record shows 6 tournaments, 450 DUSD sponsored, 200 in prizes, 250 refunded and 0 in escrow; on #5 the real-LLM Blue side has claimed the prize, and #6 has been cancelled and refunded. 3001 / 18080 keep the old demo and are not the entry point for this round's new matches.

The "Guide" page is titled "How do I get started?" and gives the first-step entry point for each role; for the full wallet account and amount walkthrough, see the step-by-step experience guide above.

Connect a wallet supported by MetaMask / RainbowKit and select the network configured by the backend `/api/config`: local Anvil (chain ID 31337) or Sepolia (11155111). In the local environment you must also check the RPC port: the same chain ID does not guarantee that you are connected to the same node. At the top of the page you can expand "Network details" to view contract addresses, confirmation count and indexing progress; "Chain data synced" indicates the service's current sync status and does not mean that a particular operation has succeeded.

The course token is DUSD, with 6 decimals and no redemption promise. The registration fee is zero, and two participants compete for the sponsored prize; there is no spectator betting. The demo administrator distributes DUSD in advance; wallets need ETH on the current network to pay network fees. The backend does not hold fund private keys.

Registration, start, review and expiry states read the latest on-chain block time, and deadlines are not judged by the browser's local clock; creation deadlines are also based on chain time. The detail page's "Prize and rules" shows the relevant deadlines and the current chain time. Without a valid deployment configuration, course transactions cannot be sent, and there is no fallback to historical contract addresses.

## Switching Between Chinese and English

Click **English** in the top toolbar to switch to English, and click **中文** to switch back to Chinese. On mobile the button is likewise directly visible, without expanding the navigation. Chinese is the default on first visit; the choice is saved in this browser and continues to apply after refreshing, closing and reopening, or visiting other pages. When the browser blocks local storage, you can still switch within the current page, but a refresh may revert to Chinese.

Switching takes effect immediately, preserves current form input and filters, and does not trigger wallet signatures or fund transactions. The wallet connection dialog also uses the selected language; the MetaMask extension's own interface is controlled by the wallet settings.

| Chinese navigation | English navigation |
| --- | --- |
| 资金概览 | Overview |
| 奖金赛事 | Tournaments |
| 审计流水 | Audit |
| 我的 Agent | My Agents |
| 使用指南 | Guide |

Interface labels, hints, dates and transaction steps are translated; tournament names, Agent names/strategies, user descriptions, raw evidence, contract event codes and raw diagnostics returned by the wallet keep their original text. When creating new content for an English demo, enter English names and descriptions yourself. A transaction that has already been estimated keeps the operation description from the time of estimation; switching language does not change fixed amounts, target contracts or signature data.

## Wallet Login and Roles

1. Click "Connect wallet" at the top. If the network does not match, click "Switch to the correct network" in the prompt, or switch manually from the top network menu or the wallet.
2. Click "Sign in". The frontend obtains a random nonce and the raw message from the server, and the wallet signature is exchanged for a login token; the message cannot be modified, and the nonce is single-use. The login signature does not transfer funds and incurs no network fee.
3. The top bar shows "Admin", "Organizer", "Auditor" or "Standard user". Standard user means having no global management role, not that you have already registered; your actual identity in a given tournament is shown separately in "Your next step" and "My tournament account" on the detail page.
4. Switching wallet or network clears the login state and requires signing again. The address menu at the top lets you view the account or disconnect; when changing demo identity, switch in MetaMask and check the address shown on the page.

The administrator expands "Administrator: manage organizer and auditor roles" on the Tournaments page, selects the role and target wallet, clicks "Preview role grant" / "Preview role revocation", and then confirms and signs. The organiser creates tournaments, starts them, runs matches, submits results or cancels before the start; the designated auditor reviews results and rules on disputes; ordinary wallets can create Agents, register, sponsor and claim their own funds. The designated auditor cannot sponsor a tournament they review. The wallets of the tournament organiser, the designated auditor and the two participants must not conflict with one another; permissions are ultimately validated by the contracts.

## Common Confirmation Steps for All On-chain Operations

1. Click the specific business button, such as "Register and review fees", "Start match" or "Claim 100 DUSD prize". The page automatically opens the transaction confirmation dialog, which first simulates and estimates.
2. Check "Operation" and the amount, wallet and tournament information in it, then look at "Estimated network fee". The ETH network fee is different from the DUSD amount in the business operation. If needed, expand "Transaction details" to check the target contract, chain ID and signing account.
3. Click the large "Confirm and sign in wallet" button, then confirm in the wallet. The first click on a business button does not send a transaction automatically.
4. Wait for "Transaction confirmed", then click "Done". The page and the confirmed ledger may need to be refreshed a little later.

Operation parameters are fixed at estimation; after changing the form, click the business button again and re-estimate. The network fee is based on the chain state at that time, and the actual fee is determined by the receipt.

Escape or "Hide window" closes the dialog; this neither resets the transaction nor cancels a transaction already sent. While pending, the bottom of the page keeps a "View transaction progress" or "View and check receipt" entry point. Do not resubmit while signing, confirming or when the result is unclear; you can click "Check transaction receipt" to verify the final state. Rejecting the signature does not send a transaction; an execution revert shows the reason and cannot be counted as a successful payment.

## Agent Definitions and Registration

1. Open "My Agents", fill in a name under "Create an agent to compete", choose "Aggressive" or "Defensive", and optionally fill in "Strategy and personality". Click "Save agent and prepare to compete".
2. Saving does not require an on-chain transaction. Definitions cannot be modified; the backend persists the name, strategy, owner and content hash; "View definition fingerprint" expands the hash. To change strategy, create a new definition. The platform configures the model centrally and does not collect personal model API keys or arbitrary API URLs.
3. Under "Saved agents", click "Compete with this agent", or open the detail page of a tournament still in its registration period from "Tournaments".
4. Under "Participate in this tournament", choose "Compete", select an Agent you created, click "Register and review fees", and sign following the common confirmation steps. On success you will see the registration count and your own Agent.
5. Before the registration deadline you can click "Withdraw from this tournament"; once the transaction is confirmed the slot is freed, and you can choose an Agent and register again. After the deadline you can no longer register, sponsor or withdraw.

Historical #1 uses deterministic simulation and only recognises keywords such as `aggressive` / `defensive`; the current 3002 environment has a real LLM configured, and #5 retains a record of 14 rounds and 28 successful calls. "Deterministic simulation allowed" and "model configured" are both service configuration; the actual mode of a given match and whether calls succeeded must be checked in "Match replay", the call statistics and the evidence.

## Creation, Sponsorship and Start

1. The organiser clicks "Create tournament" on the Tournaments page, expands it and fills in the name, the designated auditor wallet, an optional rules link and the deadlines; by default, registration closes in 15 minutes, the final settlement deadline is in 24 hours, and the review window is 60 seconds. The review window ranges from 60 seconds to 30 days; the final deadline must leave the full review time. Click "Preview and create tournament", check and sign according to the dialog, and the tournament appears in the list once confirmed and indexed. "Close creation form" only collapses the input area.
2. During the registration period, a sponsor opens "Participate in this tournament" on the detail page, chooses "Sponsor prize" and enters a DUSD amount (up to 6 decimals); the 25 / 100 / 200 DUSD quick-select amounts can also be used.
3. Click "1. Approve 100 DUSD" (the amount follows the input), confirm and sign, and wait for the approval to succeed. The approval spender is the configured PrizeEscrow, not TournamentManager; the approval itself does not deduct funds or increase the prize pool.
4. Once sufficient approval is shown, click "2. Confirm sponsorship", then confirm and sign again. After the sponsorship transaction succeeds, "Total tournament prize" and "My sponsorship · Held in escrow" increase, and the available wallet balance decreases.
5. After registration has closed, with two participants, a non-zero prize and enough time remaining for review, the organiser clicks "Start match" under "Your next step" and signs according to the dialog. Even with two participants, you must wait for the registration deadline and cannot start early.

## Matches, Evidence and Result Review

1. While the tournament is in the match stage, the organiser clicks "Run Agent match". This is a backend job and does not require sending another on-chain transaction; the backend persists rounds and evidence and never submits the result on behalf of the wallet. A failed real request or parse interrupts the match, without fabricating rounds or a winner; after checking the error, "Resume match execution" can be clicked only if the backend allows a retry, and historical failures and the call budget are still kept. A completed match cannot be run again.
2. "Match replay" shows the map, HP, actions and the actual run mode. You can "Play replay", pause, step to the previous/next round or drag the slider; "Follow latest round" restores the latest state. "View round actions and decision records" expands the detailed actions.
3. The organiser clicks "View match evidence", or goes to "Match evidence" and clicks "Show full evidence" / "Download evidence JSON". The browser checks the Keccak-256 of the canonical JSON against the returned evidence hash and match result hash, and shows "Content verification passed" and "Viewed".
4. When the match is complete, has a winner, the evidence matches and there is still the full review time, the organiser clicks "Submit match result" and signs according to the dialog to submit the winner address and evidence hash. A draw must not fabricate a winner; wait for expiry cancellation and refunds.
5. After the result is submitted, "Review window open" is shown. A participant or an actual sponsor can, within the window, expand "Disagree with the result? Submit a dispute", fill in "Grounds for dispute", click "Submit dispute and review fees" and sign. The explanation saved by the backend is only a draft; the on-chain dispute takes effect only when the corresponding hash appears in a confirmed event.
6. After the full review window ends, the designated auditor first views the evidence. If there is no dispute, click "Approve result and confirm winner". If there is a dispute, click "Review dispute and decide", choose a winner or "Reject result: cancel tournament and enable refunds" under "Dispute decision", fill in "Reason for decision", click "Submit decision and review fees" and sign. Confirming a winner requires evidence that has been viewed and matches; choosing to reject and cancel with refunds does not require the original evidence to be complete, but still requires the designated auditor role, the end of the full review window and a reason. Review settlement is not allowed once the final deadline has been reached.
7. After submission, the evidence area should show "Content matches the on-chain record". A matching hash only proves that the content commitment matches; the auditor still needs to judge whether the rules and match result are correct.

## Prize Claims, Cancellation and Refunds

- "Settled · Ready to claim" means the review is complete but payment has not yet been made. The winner connects their own wallet and clicks "Claim 100 DUSD prize" under "Your next step" (the amount follows the tournament's prize); after a successful signature, "Prize claimed" is shown. It can only be claimed once.
- Before the start, the organiser can expand "Need to cancel this tournament?", fill in "Reason for cancellation", click "Cancel tournament and review fees" and sign. An auditor ruling that rejects the result also cancels the tournament.
- When registration has closed with too few participants, or the final settlement deadline has been reached, any wallet can click "Cancel tournament and enable refunds". This path remains valid without depending on management roles.
- "Expired · Refund pending" does not mean cancelled, let alone that the funds have been returned. First confirm the cancellation, then each sponsor connects the original funding wallet and clicks the button for the corresponding amount, such as "Claim 200 DUSD refund". Each sponsor claims independently; there is no batch transfer by the backend or the organiser.
- "Cancelled" only means the tournament is cancelled, not that all sponsors have been refunded. "My tournament account" lets you check "My sponsorship awaiting refund", "Claimable refund" and "Available wallet balance"; confirm receipt by your own balance increase and the refund event.

## Ledger and Troubleshooting

Overview shows confirmed sponsorship, prizes paid, sponsorship refunded, funds pending settlement and indexing progress. Funds pending settlement equal cumulative sponsorship minus prizes and refunds. Audit queries events by tournament, and you can filter the current page or "Export this page as JSON"; search only filters this page, and the export keeps all raw records of this page — it is not a full-database export. Sepolia transactions provide Etherscan links, and the local network shows the transaction ID. The tournament selector shows the latest 100 tournaments, with 50 events per page.

When you encounter a problem, first check the address at the top, the wallet network, the sign-in status and the tournament's "Your next step". If the service connection fails, click "Reconnect"; if chain data is not yet ready, click "Check again"; if the network does not match, click "Switch to the correct network". Waiting for indexing does not mean the transaction needs to be repeated; if the receipt has succeeded but the ledger has not yet updated, refresh later and check the indexing progress in "Network details".

Tournament status is distinguished by chain time into registration open, awaiting start, review window, ready to claim and expiry refund. If "Current chain time" has not been read successfully, check the node first; do not use the computer's time to assume a deadline has passed. Amounts, states and permissions are still ultimately validated by the contracts; the page never treats an operation that has not been signed or confirmed as successful.
