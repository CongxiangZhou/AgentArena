# Agent Arena Walkthrough Demo Script

English synthesised voice-over · English and Chinese subtitles in the published video · Tournament #13.

## 00:00 · Agent Arena

Welcome to Agent Arena: an AI tournament platform with transparent prize escrow and independent result review.

Let's follow a complete tournament, from preparation to payment.

## 00:12 · A fast local demonstration

This demonstration runs locally for fast responses and a smooth, complete walkthrough.

MetaMask signs transactions on the local blockchain, and DUSD is our demonstration token.

The homepage introduces the prize pool, Agent competition, independent review, and final claim.

## 00:32 · Clear responsibilities

Small tournament organizers need clear rules and a reliable way to distribute sponsored prizes.

Here, organizers set the rules, developers enter Agents, sponsors fund the pool, and an auditor reviews the result.

## 00:48 · Follow the funds

The dashboard separates sponsorship received, prizes paid, refunds, and funds still held in escrow.

Now let's create and run tournament thirteen. Account switching and idle waits are shortened in this edit.

## 01:02 · 01 / Create the tournament

The organizer signs in with MetaMask and opens the tournament creation form.

We enter a title and designate the auditor who will check this competition's result.

The form sets the registration deadline, final settlement deadline, and result review window.

For this match, registration lasts four minutes, settlement allows twenty-four hours, and review lasts sixty seconds.

The transaction preview displays the action and estimated gas before MetaMask confirmation.

After confirmation, tournament thirteen is created with its rules and auditor recorded on chain.

## 01:40 · 02 / Prepare the red Agent

Next, the red participant opens My Agents and selects the saved MetaMask Demo Red profile.

An Agent profile contains its name, character, and instructions for deciding how to play.

This is where a developer expresses a strategy: when to approach, attack, use a skill, or heal.

The saved definition has a content hash, which links this exact Agent to its tournament entry.

## 02:08 · 03 / Register the red participant

The participant follows the tournament link, chooses the red Agent, and previews registration.

Registration is free. The wallet signs the entry, including the selected Agent definition hash.

The confirmed participant card now shows the red wallet and its registered Agent.

## 02:29 · 04 / Register the blue participant

The blue participant signs in with a separate wallet and selects MetaMask Demo Blue.

The tournament page gives each role a clear next action as the competition progresses.

The blue participant reviews and signs the second registration in MetaMask.

Both participants are registered, with their Agent definitions linked to their wallets.

## 02:52 · 05 / Sponsor 100 DUSD

The sponsor signs in and chooses to contribute one hundred DUSD to this tournament.

There are two wallet transactions. First, approve the escrow contract to use exactly one hundred tokens.

Second, confirm the sponsorship deposit, which transfers the tokens into the tournament's escrow.

The confirmed prize pool now holds one hundred DUSD.

Funding is now in place before the competition begins.

## 03:26 · 06 / Start the tournament

Once registration closes, the organizer returns to a ready-to-start tournament.

Both participant slots are filled and the sponsored prize is held in escrow.

The organizer confirms Start match in MetaMask, moving the tournament into its active state.

## 03:46 · 07 / Launch the real AI match

Run Agent match now starts the backend battle engine using the two registered Agent definitions.

This match uses the Qwen three point five Plus language model for decisions.

## 04:00 · 08 / Watch the battle unfold

Each round, the Agents receive the current arena state and choose their next actions.

The game engine checks those actions against movement, attack, healing, and obstacle rules.

As the battle develops, the board shows positions, while the health bars show the effect of each exchange.

The Agents close the distance and trade attacks. Their decisions are recorded alongside the resulting game state.

At low health, healing becomes part of the strategy. The outcome follows the executed actions and game rules.

The live display keeps updating until the engine reaches the final result.

## 04:40 · 09 / Inspect the result

After fifteen rounds, the blue Agent wins with ten health points remaining.

The action history shows each round, and the evidence view brings together the Agents, actions, and final result.

The complete evidence can also be downloaded as JSON.

## 05:00 · 10 / Replay the match

The replay lets us revisit the battle after it has finished.

Play the recorded sequence or move between rounds to inspect positions, health, actions, and decision records.

This gives participants and the auditor a concrete record to examine before the result is approved.

## 05:22 · 11 / Submit the result on chain

The organizer reviews the evidence, then submits the blue winner and the evidence hash.

The wallet preview makes the proposed winner and commitment visible before signing.

After confirmation, the tournament enters its result review window.

The prize remains in escrow while the designated auditor checks the result.

## 05:47 · 12 / Independent audit

We now sign in with the auditor's wallet, which has a separate responsibility from the organizer.

The auditor inspects the submitted winner, the match record, and the evidence associated with this tournament.

The evidence hash connects this stored content to the commitment submitted on chain.

The review step gives a dedicated role control over approving the settlement result.

## 06:13 · 13 / Approve the settlement

After checking the evidence and reaching the review deadline, the auditor approves the result through MetaMask.

The tournament is finalized, and the blue winner can now claim the sponsored prize.

## 06:31 · 14 / Claim the prize

The blue participant reconnects and signs in as the approved winner.

The page displays a claimable prize of one hundred DUSD.

The winner previews Claim prize and confirms the transfer in MetaMask.

The escrow contract pays the eligible wallet and records that this tournament's prize has been claimed.

This completes the final financial step of the tournament.

## 06:59 · 15 / Verify the payout

The winner's displayed balance has increased from three hundred to four hundred DUSD.

This tournament's escrow is now zero, and its status reads Prize claimed.

## 07:14 · 16 / One traceable financial history

The audit page ties the workflow together: creation, registrations, sponsorship, match start, result submission, approval, and prize claim.

Each confirmed event links the action to its transaction record.

## 07:30 · 17 / Cancellation and refunds

The platform also supports cancellation and sponsor refunds. Here we are viewing a historical refund example.

Tournament six records a twenty-five DUSD deposit, cancellation, and refund to the sponsor.

This section only displays the saved history.

## 07:48 · 18 / Full-stack architecture

The workflow connects four layers: a Next.js frontend, a Go backend, SQLite storage, and Solidity smart contracts.

The interface uses wagmi and RainbowKit to connect MetaMask and guide each transaction.

Wallets sign blockchain operations, while REST APIs provide the profiles, match execution, and dashboard data.

## 08:12 · 19 / Interacting smart contracts

Tournament Manager coordinates three dedicated smart contract modules.

Tournament Registry stores registration, deadlines, and lifecycle state.

Prize Escrow holds sponsored tokens and handles prize claims and refunds for each tournament.

Result Review manages submitted evidence, review windows, and dispute decisions. DUSD provides the ERC-twenty token used for settlement.

## 08:38 · 20 / Backend and persistent records

The Go service uses Gin for its REST endpoints and SQLite for persistent application records.

Agent definitions, round histories, match evidence, and review notes are stored for later inspection.

A blockchain reader indexes confirmed events into the database, making financial history available to the dashboard.

## 09:00 · 21 / From Agent decisions to evidence

The Agent chooses an action from the current game state, and the engine applies deterministic game rules.

Saved rounds power both the replay and the complete evidence record.

Registration binds the Agent definition hash; result submission anchors the evidence hash on chain.

The auditor then connects the recorded competition to the approved settlement outcome.

## 09:24 · 22 / Security by responsibility

Wallet login uses a single-use nonce, and smart contracts enforce the required roles for each operation.

Escrow transfers use safe token handling and reentrancy protection, with separate accounting for each tournament.

Claim checks prevent duplicate payouts. The winner requests payment from their own wallet after approval.

These controls support a clear and traceable financial process.

## 09:48 · A complete tournament, from start to settlement

One funded tournament, two language model Agents, independent review, and one hundred DUSD successfully claimed.

That is Agent Arena, from sponsorship to settlement.
