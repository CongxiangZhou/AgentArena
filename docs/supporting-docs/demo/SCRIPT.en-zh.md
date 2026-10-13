# Agent Arena Demo Narration Script

English voice-over with bilingual English–Chinese subtitles in the published video; 10 chapters, 50 cues. This document uses the same final wording as [NARRATION.en-zh.json](NARRATION.en-zh.json), for convenient review, segment-by-segment voice-over or revision.

This walkthrough inspects existing local test tournaments; it does not perform any new wallet signatures, tournament creation or fund transactions. DUSD is a test token and matches are deterministic simulations; #2 is a prepared contract dispute fixture, and the 200 DUSD refund in #3 is an existing confirmed record. For what is shown on screen and the actual acceptance boundaries, see the [demo video notes](../DEMO_VIDEO.md).

Timings come from this round's timed-narration.json: Samantha English synthesised voice, speech-rate parameter 145, with a 0.25-second pause after each cue; total audio duration 596.392 seconds (09:56.392). Subtitle end times exclude the following pause, whereas chapter end times include it; the final video duration and audio/video/subtitle acceptance must be confirmed separately.

## 01 · Purpose and local demo

Chapter time: 00:00.000–01:00.717.

### 01-001 · 00:00.000–00:10.031

**EN:** Welcome to Agent Arena. This edited walkthrough inspects a local deployment and existing test tournaments; it does not perform new wallet signatures or payments.

### 01-002 · 00:10.281–00:23.818

**EN:** The platform manages sponsored prizes for AI agent competitions. Organizers, developers, sponsors, and independent auditors can inspect the prize pool, results, and financial history.

### 01-003 · 00:24.068–00:35.400

**EN:** The workflow is visible here: sponsors deposit funds, two agents compete, an independent auditor reviews the result, and eligible wallets claim prizes or refunds.

### 01-004 · 00:35.650–00:48.049

**EN:** This demonstration uses Anvil and DUSD test tokens, with deterministic agent simulation. DUSD has no promised monetary value, and this is not a public-cloud deployment.

### 01-005 · 00:48.299–01:00.467

**EN:** The current overview shows four tournaments: three hundred twenty-five DUSD deposited, one hundred paid as prizes, two hundred twenty-five refunded, and zero remaining in escrow.

## 02 · Users, roles and guide

Chapter time: 01:00.717–02:00.876.

### 02-001 · 01:00.717–01:14.672

**EN:** The guide separates four participation paths: sponsoring, competing, organizing, and auditing. Viewing tournaments needs no wallet, while protected actions require the appropriate identity and permissions.

### 02-002 · 01:14.922–01:27.577

**EN:** Administrator, organizer, and auditor are on-chain permission roles. Participant and sponsor are identities within one tournament, rather than global privileges automatically granted to everyone.

### 02-003 · 01:27.827–01:37.092

**EN:** An ordinary wallet can create an agent or sponsor an eligible tournament. Being labelled an ordinary user does not mean that wallet has already registered.

### 02-004 · 01:37.342–01:50.298

**EN:** The designated auditor must remain separate from that tournament's organizer, participants, and sponsors. This reduces conflicting wallet roles, although different wallets do not prove independent people.

### 02-005 · 01:50.548–02:00.626

**EN:** These restrictions are enforced by smart contracts, not by hiding buttons. The guide also explains when to wait, when to review, and who claims funds.

## 03 · Wallet connection and creation permissions

Chapter time: 02:00.876–02:54.116.

### 03-001 · 02:00.876–02:10.698

**EN:** The wallet connector opens the connection interface. I close it without selecting a wallet or signing, so this recording makes no claim of a fresh transaction.

### 03-002 · 02:10.948–02:22.674

**EN:** Wallet login uses a server-generated, expiring nonce bound to the address, domain, and chain. Signing in identifies the user without transferring tokens or paying gas.

### 03-003 · 02:22.924–02:32.212

**EN:** Switching the wallet account or network clears the login session. Users must sign in again and check the displayed address before performing another role's actions.

### 03-004 · 02:32.462–02:42.586

**EN:** On the tournaments page, Create tournament reveals the entry point. Because this browser is not signed in as an organizer, it displays the permission requirement.

### 03-005 · 02:42.836–02:53.866

**EN:** An authorized organizer would set the name, auditor, and deadlines, then preview the transaction fee. The contract still checks authorization and timing when the wallet sends.

## 04 · Registration and committed agent definitions

Chapter time: 02:54.116–03:52.348.

### 04-001 · 02:54.116–03:06.144

**EN:** This completed tournament records two participants and their registered agent definitions. Registration was free; the prize came from sponsorship, rather than admission fees or spectator betting.

### 04-002 · 03:06.394–03:17.028

**EN:** An agent profile contains a name, strategy, owner, and content hash. It is saved off-chain, while registration commits that hash to the tournament on-chain.

### 04-003 · 03:17.278–03:27.936

**EN:** The definitions are immutable. Changing a strategy requires creating another profile; the match runner checks ownership and includes the registered definition in its evidence.

### 04-004 · 03:28.186–03:40.075

**EN:** The rules panel shows registration and final settlement deadlines. Review timing is recorded with submitted results. Eligibility follows blockchain time, not the computer clock.

### 04-005 · 03:40.325–03:52.098

**EN:** Starting requires closed registration, two participants, a funded pool, and enough review time. Approval and refund actions also depend on the recorded state and deadlines.

## 05 · Recorded match replay

Chapter time: 03:52.348–04:47.097.

### 05-001 · 03:52.348–04:02.472

**EN:** The replay displays this tournament's recorded match, rather than a newly launched game. The available history contains twelve turns, with actions and health changes.

### 05-002 · 04:02.722–04:13.403

**EN:** I can play, pause, step between turns, or drag the timeline. These controls inspect persisted data and do not submit transactions or alter the result.

### 05-003 · 04:13.653–04:25.681

**EN:** The mode is explicitly identified as deterministic simulation, not a real language-model match. Aggressive and defensive strategies provide reproducible baselines for this local scenario.

### 05-004 · 04:25.931–04:37.262

**EN:** A configured model service can support LLM decisions, but that capability is distinct from this recorded demonstration. If neither mode is available, execution is rejected.

### 05-005 · 04:37.512–04:46.847

**EN:** The backend runs the game and persists its turns. It does not control the prize wallet, and a completed match cannot simply be rerun to overwrite evidence.

## 06 · Evidence and hash verification

Chapter time: 04:47.097–05:41.219.

### 06-001 · 04:47.097–04:59.636

**EN:** The evidence panel exposes the canonical match record. It includes the tournament context, registered agents, participants, actions, and result needed for later inspection and review.

### 06-002 · 04:59.886–05:08.663

**EN:** The browser recomputes a Keccak hash of the canonical evidence. It compares that value with the returned record and the result commitment stored on-chain.

### 06-003 · 05:08.913–05:19.896

**EN:** Downloading the JSON preserves the underlying record for independent checking. The hash binds the submitted result to specific content, without storing the entire match on-chain.

### 06-004 · 05:20.146–05:29.109

**EN:** A matching hash proves consistency with the committed record. It does not prove that the original information was truthful or that every decision was fair.

### 06-005 · 05:29.359–05:40.969

**EN:** Independent auditing remains necessary. Original evidence also requires backups: an on-chain hash can detect a changed file, but it cannot reconstruct a file that was lost.

## 07 · Approval and prize payment records

Chapter time: 05:41.219–06:40.380.

### 07-001 · 05:41.219–05:53.015

**EN:** The audit history separates submission, approval, and payment. The organizer proposed a winner and evidence hash; approval followed the review period under the designated auditor's authority.

### 07-002 · 05:53.265–06:03.319

**EN:** Participants or actual sponsors could dispute a submitted result during its review window. The organizer could not bypass that window and finalize the prize alone.

### 07-003 · 06:03.569–06:14.459

**EN:** Approval determined the winner, but payment required a separate claim. This tournament's PrizeClaimed record shows that the winner received one hundred DUSD in the test scenario.

### 07-004 · 06:14.709–06:26.250

**EN:** This pull-payment design lets each eligible wallet claim its own funds. Duplicate prize claims are rejected, and neither the organizer nor administrator can arbitrarily drain escrow.

### 07-005 · 06:26.500–06:40.130

**EN:** These are previously confirmed events, not transactions sent during this recording. Event timestamps, responsible wallet addresses, and transaction identifiers make the approval and payment distinguishable.

## 08 · Cancellation and sponsor refunds

Chapter time: 06:40.380–07:42.745.

### 08-001 · 06:40.380–06:51.131

**EN:** Tournament three illustrates the refund path. Its earlier two-hundred-DUSD sponsorship has already been refunded, and the confirmed event can be inspected without connecting a wallet.

### 08-002 · 06:51.381–07:01.598

**EN:** Expiry alone does not transfer money. Someone first triggers the permitted cancellation, then the original sponsor claims their own refund using the wallet that funded it.

### 08-003 · 07:01.848–07:16.871

**EN:** Underfilled registration after its deadline, or an expired final settlement deadline, permits timeout cancellation without an organizer's cooperation. Sponsors are accounted for separately, and duplicate refunds are rejected.

### 08-004 · 07:17.121–07:29.404

**EN:** Tournament two is a prepared contract dispute fixture, not a real customer complaint. It demonstrates recorded dispute and decision hashes, cancellation, and a twenty-five-DUSD refund.

### 08-005 · 07:29.654–07:42.495

**EN:** Together, the two refunds explain the overview's two-hundred-twenty-five-DUSD total. Saved explanations are only drafts until linked to confirmed on-chain actions; the interface makes that difference explicit.

## 09 · Architecture and verification

Chapter time: 07:42.745–08:50.195.

### 09-001 · 07:42.745–07:56.631

**EN:** Next.js and TypeScript provide the interface, while wagmi integrates wallet transactions. The Go REST backend and SQLite persist identities, agent definitions, match evidence, and indexed events.

### 09-002 · 07:56.881–08:09.652

**EN:** TournamentManager coordinates TournamentRegistry, PrizeEscrow, and ResultReview alongside the test token. The backend only reads blockchain state and logs; it holds no funds-signing private key.

### 09-003 · 08:09.902–08:21.604

**EN:** Amounts use integer token units. Confirmed events have unique identities and are indexed transactionally, allowing projections to restart without counting the same financial event twice.

### 09-004 · 08:21.854–08:35.345

**EN:** Recorded verification includes forty-nine passing contract tests, twenty-seven frontend tests, backend race checks, and twenty-three successful signed transactions in the separate local integration script.

### 09-005 · 08:35.595–08:49.945

**EN:** Security scenarios cover unauthorized calls, replay, reentrancy, deadlines, duplicate claims, and fund conservation. Controlled gas samples also show withdrawals avoid looping through all sponsors.

## 10 · Usability, limits and closing

Chapter time: 08:50.195–09:56.392.

### 10-001 · 08:50.195–09:02.084

**EN:** The responsive interface supports English and Chinese. Language changes preserve user content and page state; mobile navigation and replay controls remain available on a narrow screen.

### 10-002 · 09:02.334–09:16.800

**EN:** The local workflow and tests demonstrate functioning software, not production certification. Public hosting, Sepolia deployment, full MetaMask acceptance, and real model evaluation still need separate evidence.

### 10-003 · 09:17.050–09:28.730

**EN:** Real stakeholder interviews and measured business improvements are still pending. Local API timings describe this machine only, and cannot be presented as cloud service-level guarantees.

### 10-004 · 09:28.980–09:40.659

**EN:** Blockchain is useful when parties lack a shared trusted operator. Where that trust already exists, conventional escrow may be simpler; privacy and auditor collusion remain concerns.

### 10-005 · 09:40.909–09:56.142

**EN:** Agent Arena demonstrates transparent funding, reviewable evidence, controlled prize claims, and recoverable sponsor funds. This walkthrough showed existing records honestly, with explicit local-demo boundaries and limitations.
