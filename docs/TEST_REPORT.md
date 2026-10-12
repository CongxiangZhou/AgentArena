# Course Version Test and Evaluation Record

## Current release status

Standalone repository: https://github.com/aokamutidao/agent-arena-escrow (`main`). The test records below correspond to the actual code and dates; historical local results are not described as cloud load tests.

- Final pre-release frontend checks: 42/42, typecheck, lint and production build all passed.
- Foundry: 49 tests passed, comprising 41 course tests and 8 historical security regressions.
- Gas figures are taken from the committed `contracts/snapshots/PrizeScale.json` and `RefundScale.json`: 66,162 / 63,976, identical across all three sponsor counts; other measurement contexts in historical logs are not mixed in directly.
- The final [600-second hands-on video validation](supporting-docs/demo-user-recording/VALIDATION.json) passed.
- The [Sepolia deployment](supporting-docs/evidence/sepolia-deployment-2026-10-10.json) includes four successful transactions, code for five contracts and checks of three roles.
- Both the frontend and backend Render services are Live, actually running main `9e8e429`. The [public smoke test](supporting-docs/evidence/cloud-smoke-2026-10-10.json) passed: HTTPS home page, API /health /ready /api/config, the five Sepolia contracts and synchronised data; the Chrome home page, language switching and the Installed MetaMask connection entry passed. No wallet signatures or cloud business transactions were performed.
- All [four main CI jobs](https://github.com/aokamutidao/agent-arena-escrow/actions/runs/38061976954) in this repository succeeded; the [raw CI record](supporting-docs/evidence/github-ci-2026-10-10.json) preserves the same version and results.

Initial test date: 2026-10-09; subsequent checks are listed by round as recorded on 2026-10-10. Environment: local macOS / Go 1.25.4 / Node.js 24.11.1 / Foundry 1.5.1 / Solidity 0.8.24 (optimizer 200). This report distinguishes executed evidence from items still awaiting acceptance. It is not an external security audit, nor is it equivalent to production deployment certification.

## 2026-10-10: MetaMask main tournament acceptance (034, final scope passed)

A [pre-acceptance snapshot](supporting-docs/evidence/metamask-before-2026-10-10.json) was saved: API `18081` ready, frontend `localhost:3002`, RPC `18545` / chain ID `31337`. The six tournaments and balances in the baseline reflect the state before operations began. The initial stopping point, where the extension confirmation page could not be operated, was resumed; the browser tool explicitly refuses access to `chrome-extension:`, so the user personally operated the MetaMask accounts and confirmations, while the assistant only operated this project's localhost pages; CLI test private keys were not used in place of wallet signatures in this round.

| Stage check | Result that occurred |
| --- | --- |
| Login and Agent | The Red and Blue accounts were each logged in with the user's confirmation; the web page saved `MetaMask Demo Red` / `MetaMask Demo Blue`, and the owners and registration hashes were verified |
| #7 wallet transactions | Creation, ERC20 approval, 100 DUSD sponsorship, Red registration, Blue registration, start, submission, review and prize claim: 9 successful receipts in total, all independently verified; total gasUsed 1,195,013. Covers 7 kinds of tournament business operation plus 1 kind of ERC20 approval; the approval is not a deposit, and the two participants' registrations are not two business types |
| Funds and start | Sponsor balance 800→700 DUSD, Escrow 0→100; both participants match their respective Agent hashes. The start receipt came 34 seconds after the registration deadline, still leaving the full review period |
| Real match #7 | Triggered from the web page with `qwen3.5-plus`: 19 rounds, 38 successful calls, 0 failed calls; Red HP 0 / Blue HP 21, Blue wins. Total 39,651 tokens (input 38,993 / output 658) |
| Duration and actions | The saved match timestamps differ by 43 seconds (second precision); the cumulative call time for both sides is 64,071 ms, which is not wall-clock time because the calls run in parallel. The engine recorded one SKILL by Red in round 4 that failed because an obstacle blocked line of sight; 0 failed model calls does not mean every action succeeded |
| Evidence | Schema 2 canonical hash, match snapshot, owners of both Agents, and the linkage of 19 rounds with 38 successful calls were verified; the hash is `0x9d07e71790db88b9e531e42e74e3742d36176170ab3c067512e40554787876aa` |
| Submission and review | The on-chain submitted hash matches the verified evidence; the review window was 60 seconds, and the designated reviewer reviewed 138 on-chain seconds after submission (78 seconds after the review deadline), with the result entering Finalized. The operation log records natural waiting without any time fast-forward calls; the receipt check does not audit the full history of clock-control RPC calls |
| Prize claim | Blue personally confirmed in MetaMask; the web page, the Transfer/PrizeClaimed events and the balances at the fixed receipt block agree: Blue 200→300 DUSD, escrow balance and liabilities 100→0, prizeClaimed=true. Transaction `0xc2e5d23962d2c2666912afab07d2d0cf510cf039eda670f73e52ad5ba8d2a933`; a repeat claim was tested only via a read-only eth_call and was rejected as expected; no further transaction was sent |
| Frontend fixes | Fixed the reproduced stale Agent save message and the SSR hydration bug that wrongly deleted the login session. Added 4 Agent and 8 auth regression tests; the final full suite passed 42/42, and typecheck, lint on the relevant files and the production build (including full lint/type checks and generation of 15 pages) passed; runtime-v4 has been deployed on 3002, keeping the API, chain and database. The new Agent message implementation has only completed automated regression testing; the cross-account save browser operation was not repeated separately |
| Session restore on refresh | A valid old v3 organiser session was reloaded into v4; after the initial connection prompt, Organizer, Sign out and `0x70…79C8` were restored automatically with no new login signature; the live browser re-check passed, and the screenshot is stored temporarily at `.local/llm-demo/metamask-v4-reload-auth-preserved.jpg` |

**Final scope:** The user cancelled the separate refund demonstration for this round, so 034 takes the complete #7 main tournament flow as its acceptance scope, and it has passed; no #8 was created, and no cancellation-refund or dispute wallet transactions were added. Those branches still rely on the 033 CLI / existing contract test evidence and cannot be written up as a MetaMask refund pass. The final hands-on video has been completed; the course Sepolia deployment succeeded, and public acceptance is described in the status at the top of this report. The receipt check proves the transaction outcomes; that the signing application was MetaMask comes from the user's operation and the web observation records, and cannot be determined from RPC alone; the model call records are likewise not provider bills or an external audit.

A [public evidence summary of the main tournament](supporting-docs/evidence/metamask-main-flow-2026-10-10.json) has been generated. The stage raw JSON / screenshots are stored temporarily in an ignored local directory: `metamask-ui-session.json`, `metamask-receipts.json`, `metamask-match-7-verification.json`, `metamask-claim-success.jpg`, `metamask-paid-complete.jpg`. The final frontend test and build logs are `.local/llm-demo/034-auth-tests.txt` and `034-frontend-v4-build.txt`; the logs of the earlier 34-test version are still kept as `034-frontend-tests.txt` and `034-frontend-build.txt`. For detailed receipts, the hydration fix and the runtime-v4 refresh re-check, see the 034 record (the historical record is kept in the original development repository). #5/#6 from 033 remain CLI acceptance.

## 2026-10-10: Real model and local funds loop (033)

This round created main tournament #5 and cancellation-refund tournament #6, continuing to use the existing course contracts on Anvil 31337. The new API `18081` uses an online backup of the original SQLite database, and the frontend entry point is `http://localhost:3002/tournaments/5`; the original chain was not reset. The actual business signatures came from CLI scripts using public Anvil development accounts; **the script-based acceptance was not recorded as MetaMask pop-up operation or as a new video recording**.

| Acceptance | Actual result |
| --- | --- |
| Model connectivity | 4 actual diagnostics: the first 3 returned HTTP 400; the redacted errors confirmed that the two Coder model names are not supported by the current endpoint. The 4th, `qwen3.5-plus` / `enable_thinking=false`, returned HTTP 200, a valid WAIT, 43 tokens, 1,468 ms |
| Real match #5 | 14 rounds, 28 actual model calls, 28 successful, 0 failed, no retries; Blue won naturally, Red HP 0 / Blue HP 25; no deterministic simulation or preset winner |
| Model usage and duration | Provider-reported input 28,480 / output 483 / total 28,963 tokens; match wall-clock time 30 seconds (timestamp precision 1 second). The cumulative request time for both sides is 41,484 ms, which is not wall-clock duration because the requests run in parallel |
| Evidence | Schema 2; each round is linked to two successful calls. The canonical JSON Keccak-256 matches the result hash submitted on-chain |
| Prize #5 | Create, sponsor, register/withdraw/re-register, start, submit, review, claim; Blue's DUSD balance rose from 100 to 200, an actual receipt of 100 |
| Refund #6 | A separate new tournament was sponsored with 25 DUSD and then cancelled; the sponsor's balance rose from 775 to 800, an actual refund of 25; no second model match was run |
| Wallet transactions | 14 successful receipts this round, covering 10 business transaction types, total gas 1,924,746; the final escrowed token balance and total liabilities are both 0 |
| Time windows | To shorten the local registration wait, one 164-second Anvil time fast-forward is explicitly recorded; after the result was submitted, the full 60-second review window was waited out, with no fast-forward of the review window |
| Backend regression | Full test / vet passed; race for agent and tournament passed with no DATA RACE. 10 new top-level tests cover failure interruption, cancellation, strict actions, retry/restart budgets, redaction, and byte/hash compatibility with the old schema 1 |
| Frontend | 30/30 unit tests, type checking, lint and the production build passed; the real page automatically updates calls and rounds, evidence verification passed, and the prize-claimed and refunded states can be viewed |
| Mobile and bilingual | At 390 × 844 the page width is 390, with no full-page horizontal overflow; the English call statistics are readable, and after switching back to Chinese the selected round 13 remains unchanged |
| Archiving | Online SQLite backups and Anvil state snapshots from before and after the operations are kept in `.local/llm-demo/`; a backup restore drill was not performed |

Evidence: [connectivity](supporting-docs/evidence/llm-connectivity-2026-10-10.json), [local business operations and balances](supporting-docs/evidence/local-llm-integration-2026-10-10.json), [full match evidence](supporting-docs/evidence/local-llm-match-5-2026-10-10.json), [backend tests](supporting-docs/evidence/backend-llm-tests-2026-10-10.txt), [race](supporting-docs/evidence/backend-llm-race-2026-10-10.txt), [frontend tests](supporting-docs/evidence/frontend-llm-tests-2026-10-10.txt), [build](supporting-docs/evidence/frontend-llm-build-2026-10-10.txt), [mobile view](supporting-docs/evidence/llm-mobile-2026-10-10.jpg). `go vet` exited 0 with no diagnostic output; race retains the pre-existing macOS linker warning, and the frontend retains the WalletConnect optional `pino-pretty` warning.

The Dashboard snapshot at the end of 033 shows 6 tournaments, cumulative deposits of 450 DUSD, prizes of 200 and refunds of 250; this includes old demonstrations, so not all of it can be attributed to 033. The new deposits in this round are 125, prizes 100 and refunds 25 DUSD. The new transactions from the subsequent 034 are shown at the very start of this report.

This round involved 4 diagnostics + 28 match model requests in total. Successful calls and the local test funds loop do not mean that the plan permits general application-backend use, nor do they represent public deployment, real-asset payments, provider signature proofs or an independent external audit. Cloud preparation continues on the confirmed Render free plan; the course public URL, testnet deployment and the new hands-on video with English voice-over were not completed in this round.

Reproduction and code reading: [local real-model guide](supporting-docs/LOCAL_LLM_DEMO.md), 033 development record (the historical record is kept in the original development repository).

## 2026-10-10: Video with English narration and bilingual subtitles (032)

A [9 min 56 s demo video](supporting-docs/DEMO_VIDEO.md) has been produced, containing actual local interface operations, a review of existing transaction records and clearly marked architecture explanation cards. This round does not change business code, does not rerun the contract/API/frontend tests, and adds no wallet signatures or fund transactions.

| Check | Actual result |
| --- | --- |
| Final video format | H.264, 1920 × 1080, 30 fps; AAC 48 kHz mono; 596.400 seconds, 30,305,416 bytes |
| Decoding integrity | FFmpeg fully decoded the final subtitled version, exit code 0, no error output |
| Voice-over and subtitles | 10 chapters, 50 sets of English voice-over / bilingual text consistent line by line; the Chinese-English, English-only and Chinese-only SRT files each have 50 entries, with no overlaps and none extending past the end of the audio track |
| Timing and volume | Audio track 596.392 seconds, audio/video duration difference 8 ms; volume detection mean −15.9 dB / peak −1.5 dB |
| Visual spot checks | The cover and frames at 14 time points were viewed; the bilingual subtitles sit in a separate bottom area and the Chinese font renders correctly; long subtitles and the refund scene were re-checked at original size |
| Generator | Python compile check, chapter reuse condition check, Chinese line-wrapping check and git diff --check passed |

Sources: [media validation JSON (including SHA-256)](supporting-docs/evidence/demo-video-verification-2026-10-10.json), [final video cover](supporting-docs/evidence/demo-video-cover-2026-10-10.png), [line-by-line study script](supporting-docs/demo/SCRIPT.en-zh.md). The video was composed from real browser screenshots at about 5 fps according to actual timing, with frames held at the end of chapters to accompany the narration; the 30 fps output is not the original capture frame rate. Frame sampling, decoding and volume checks do not replace a real viewer listening through second by second.

The narration uses the existing 4 local tournaments, 325 DUSD in deposits, 100 in prizes, 225 in refunds and 0 escrow balance; #2 is a pre-seeded contract dispute test case. The video makes clear the local network, the deterministic simulation and the unverified items, and does not present them as a new MetaMask hands-on session, a real LLM evaluation, a public deployment or stakeholder validation.

## 2026-10-10: Chinese and English interface (031)

Added a global English / Chinese toggle, saving of the browser preference and full translation of the course interface; no npm dependencies were added, the contracts/API were not modified and runtime data was not reset. `npm test` **27/27 passed** (9 added this round), `npm run typecheck`, `npm run lint` and the final production build passed, and `git diff --check` passed.

Raw output: [unit tests](supporting-docs/evidence/frontend-i18n-tests-2026-10-10.txt), [production build](supporting-docs/evidence/frontend-i18n-build-2026-10-10.txt). The build still has the pre-existing WalletConnect optional `pino-pretty` warning, which does not affect this build.

The build connects to API `18080` / RPC `18545` and only replaces the `3001` frontend; the original API PID 6178 and Anvil PID 6175 were kept. The read-only snapshot before the restart already showed **4 tournaments, 325 DUSD in deposits, 100 DUSD in prizes, 225 DUSD in refunds and 0 DUSD escrow balance**, with #3/#4 cancelled; this snapshot must not be confused with the early three-tournament data from 030. No on-chain transactions were sent in this round.

| Verification scope | Actual result |
| --- | --- |
| Language coverage | The home page, tournament list, #1 details and replay, audit, the logged-out Agent page and the guide were actually viewed in the browser; the English platform copy displays correctly, and the original Chinese tournament names are preserved |
| Preference persistence | After clicking English, navigating to the tournaments page and reloading, English is still shown; the document lang/title/description are synchronised to English |
| Wallet dialog | RainbowKit opened in English mode actually displays "Connect a Wallet" and English instructions; it was then closed without choosing a wallet or signing |
| Filter input preserved | With #3 selected in audit and `refund` entered, after switching back to Chinese the tournament and input are unchanged and still match one 200 DUSD refund record; both Chinese and English search terms are indexed as aliases |
| Replay state preserved | On the mobile details page, stepping back from round 12 to round 11 and then switching back to Chinese leaves the slider value at 10, without resetting |
| Mobile 390 × 844 | The document scrollWidth / clientWidth of the English details page and of the Chinese and English home pages are all 390; the toggle is visible, mobile navigation can jump to the home page, and long buttons are allowed to wrap |
| Keyboard and diagnostics | Enter can trigger the language toggle; no console errors were collected in the test tab during this round; the normal viewport was restored after testing |
| Semantics and integer safety | 40 English tournament scenarios cover deadlines / zero prize pool / unknown chain time / terminal states, with Chinese and English tone consistent and inputs unchanged; ordinary users are not mislabelled as participants by default; the tests preserve refund ordering, large integers and raw wallet diagnostics |

Screenshots: [English desktop home page](supporting-docs/evidence/frontend-english-home-2026-10-10.jpg), [English mobile details](supporting-docs/evidence/frontend-english-mobile-2026-10-10.jpg). Tested using browser viewports: the desktop was actually 1280 × 720; mobile used a temporary 390 × 844 viewport.

**Not executed in this round:** real wallet login/signing, the complete English flow for each logged-in role, switching language while a transaction is awaiting signature, and a real browser scenario with localStorage disabled. The Provider and transaction code were reviewed to confirm that they are not rebuilt on language change and that signing parameters are not affected by translation, but code review cannot replace manual acceptance of this part. The MetaMask extension's own language is not controlled by the platform. Descriptions of already-prepared transactions keep the language in effect at estimation time; unknown API/wallet diagnostics, user content and raw evidence are not translated automatically.

Materials: 031 development log (the historical record is kept in the original development repository), [user guide](USER_GUIDE.md#switching-between-chinese-and-english).

## 2026-10-10: Frontend usability improvements (030)

This round adjusted only the frontend and documentation, continuing to use the original 3 demo tournaments without resetting data. The final `npm test` **18/18 passed** (the original 6 + 12 presentation state / event identity tests), `npm run typecheck` and `npm run lint` both passed, and the production build succeeded. The [raw build output](supporting-docs/evidence/frontend-ux-build-2026-10-10.txt) still contains the pre-existing warning about WalletConnect's dependency on the optional `pino-pretty`; no new dependency was added to mask it.

Build parameters: `NEXT_PUBLIC_API_URL=http://localhost:18080 NEXT_PUBLIC_LOCAL_RPC_URL=http://127.0.0.1:18545 npm run build`. The new standalone service runs at `127.0.0.1:3001`; the original Anvil `18545` and API `18080` were kept, and `/ready` returns `ready: true`.

| Verification scope | Actual check result |
| --- | --- |
| Home page desktop 1280 × 900 | Clearly shows the platform's purpose, the real #1 match entry, the simulation label and 325 / 100 / 25 / 200 DUSD; see [screenshot](supporting-docs/evidence/frontend-home-2026-10-10.jpg) |
| Tournament list | #3 changed from "Registration open" to "Expired · Refund pending", #1 is "Prize claimed" and #2 is "Cancelled" |
| Replay | Clicking "Play replay" returns from the last round to the first, the control changes to "Pause", and playback actually advanced to round 12; "Follow latest round" works |
| Mobile 390 × 844 | The main navigation expands and works, and the list's creation form shows the required organiser permission; on the details page the document width and viewport are both 390, with no full-page horizontal overflow; the board and activity log keep local horizontal scrolling; see [screenshot](supporting-docs/evidence/frontend-mobile-2026-10-10.jpg) |
| Anonymous Agent entry / final activity log | "Connect your wallet, then sign in" and the connect button are visible; in the final build the #1 "Result approved" row actually shows `0x3c44…93bc`, matching the designated reviewer |
| Chrome with an ordinary wallet connected | Actually shows "Spectator", 0 personal sponsorship and a visible expiry cancellation entry, without labelling the ordinary wallet as a participant |
| Exact boundaries / large integers / event identity | New tests cover the final deadline, review window, unknown chain time, zero prize pool, terminal-state precedence, large amounts, distinguishing reviewer and winner addresses, and not guessing a missing operator wallet |

**Items not counted as passed:** No MetaMask fund signing was performed this time; the tool timed out when Chrome's native automation checked the transaction pop-up, so actually opening the pop-up, closing and restoring it, rejecting a signature, replacement transactions and the complete flow for every role still await manual re-testing. Static code checks cannot replace acceptance of this part. Expiry cancellation and refund were not operations executed this time; the original #3's 200 DUSD remains in the state it was in before the user's operation. The contracts/backend were not changed this round, and the old full-chain scripts were not rerun.

Materials: 030 development log (the historical record is kept in the original development repository), [updated experience guide](supporting-docs/EXPERIENCE_GUIDE.md).

## 2026-10-09: Overview of the original course version verification

| Layer | Actual result | Evidence / command |
| --- | --- | --- |
| Contracts | 49 tests passed, including normal business, unauthorised access / deadline / fund boundaries, and regressions for old defects | `contracts/reports/foundry-gas-report.txt` |
| Fuzz and stateful tests | Two fund fuzz tests at 256 runs each; stateful invariant 64 × 64 = 4,096 calls | As above, `contracts/test/` |
| Contract coverage | Manager lines 98.59%, branches 92.50%; Escrow/Review/Token lines 100%; Registry lines 96.72% | `contracts/reports/foundry-coverage.txt` |
| Backend | Test / race / vet for all packages and the final course-server build passed | [raw race output](supporting-docs/evidence/backend-race-2026-10-09.txt), including course and historical modules |
| Frontend | TypeScript, lint, 6 business unit tests and the production build passed; for UI checks see the later records below | `frontend/tests/course.test.cjs` |
| Local full chain | 23 successful wallet transactions; actual ERC20 payments/refunds, zero liabilities, event indexing, evidence and restart passed | [raw JSON](supporting-docs/evidence/local-integration-2026-10-09.json) |
| Formatting/scripts | Foundry fmt, `git diff --check` and Shell/Node script syntax checks passed | Reproducible with repository commands |

Old contracts and deployment scripts are also counted in whole-repository coverage, giving total line coverage of **80.54%**; local 100% contract coverage must not be written up as 100% for the whole repository. Coverage measures executed code and does not prove the absence of vulnerabilities.

## Contract verification scenarios

- Normal creation, sponsorship, registration/withdrawal, start, submission, review and prize claim.
- Organiser cancellation, timeout due to insufficient registrations, settlement timeout, independent dispute resolution and refunds.
- Non-existent tournaments, zero amounts/hashes, duplicate registrations/claims/refunds, expired operations, wrong states and permission denials.
- The designated reviewer cannot also be the organiser, a participant or a sponsor of that tournament.
- The full dispute window must be preserved; revoking roles cannot block the permissionless timeout exit path.
- Malicious ERC20 reentrancy, state rollback on payout failure, and rejection of fee-on-transfer tokens whose actual received amount does not match.
- Under random fund operations, the cumulative amount paid never exceeds the actual deposits, and contract liabilities remain consistent with the escrow balance.
- Source regressions for historical issues: side-switching bets claiming extra, deposits into invalid matches, deadlines not enforced, voter identity, and arbitrary burning.

Historical source fixes do not change the old Sepolia deployment. The new course tests only prove the behaviour of the deployment corresponding to the current local source.

## Gas: reproducible scale comparison and actual transactions

The controlled experiment fixes the same compiler configuration, has 1, 16 and 64 sponsors each fund the same tournament, and measures a single withdrawal in each case. The snapshots are in `contracts/snapshots/PrizeScale.json` and `RefundScale.json`.

| Operation (controlled contract call) | 1 sponsor | 16 | 64 |
| --- | ---: | ---: | ---: |
| claimPrize | 66,162 | 66,162 | 66,162 |
| claimRefund | 63,976 | 63,976 | 63,976 |

The conclusion is limited to this implementation and sample: claiming does not iterate over sponsors, and adding sponsors does not increase the cost of a single claim. SafeERC20, the reentrancy lock, the separate ledger and events still incur fixed costs.

Measured from independent Anvil wallet transaction receipts:

| Business transaction | gasUsed |
| --- | ---: |
| createTournament (first tournament) | 230,380 |
| fundTournament (first tournament) | 192,196 |
| startTournament | 88,526 |
| submitResult (first tournament) | 238,117 |
| approveResult | 152,699 |
| claimPrize | 136,106 |
| disputeResult | 124,506 |
| resolveDispute (cancel) | 135,170 |
| claimRefund | 114,020 |

Controlled-call gas and the gasUsed in independent transaction receipts are not the same kind of measurement: the intrinsic transaction cost, cold/warm storage access and the test call context differ, so they must not be mixed or used to claim that the difference was saved. The frontend estimates gas before each operation based on the current wallet and chain state; the actual ETH cost also depends on network fee rates.

Deployment sizes: Manager runtime 13,614 bytes, initcode 26,742 bytes; Registry 5,005, Escrow 4,515, Review 1,705, Token 2,259 bytes. See `contracts/reports/foundry-sizes.txt`.

## Actual record of the local full chain

Command: `node tooling/smoke-course.mjs` (the services kept running this time also used `--serve` and separate ports). The script starts a fresh Anvil and a separate temporary database, and signs via viem using the public local test accounts. The backend does not hold transaction private keys.

1. Deploy DUSD and the Manager, assign the organiser/reviewer roles, give the sponsor test tokens and approve Escrow.
2. Wallet nonce login; replay is rejected; ordinary participants cannot call the organiser's run endpoint.
3. Save immutable Agent definitions belonging to different wallets, sponsor 100 DUSD on-chain, register and bind the hashes.
4. Run the aggressive and defensive baselines; Blue wins after 12 rounds, with the mode explicitly `deterministic_simulation`.
5. Verify the full evidence Keccak-256, and confirm a completed match cannot be rerun to overwrite it; the organiser submits, the reviewer confirms after the window, and Blue claims 100 DUSD.
6. Inject 25 DUSD into a second tournament; use a contract dispute scenario explicitly labelled as a fixture. Save the original dispute/decision text, verify the hash and link it with the on-chain events; the reviewer rules to cancel, and the sponsor receives 25 DUSD.
7. Assert that the actual ERC20 increase in each receiving wallet is correct, and that the final Escrow balance and totalLiabilities are both zero.
8. Confirm the activity log shows deposits of 125 DUSD, prizes of 100 DUSD and refunds of 25 DUSD, 4 financial records in total; the cursor and head are both at block 27, with 21 events.
9. Stop and restart the API and reindex using the same database; funds, event count, financial record count, match evidence and the original dispute/decision text remain unchanged.

The contract addresses in the raw report correspond only to this temporary local chain and are not public addresses. `--serve` additionally creates a third, open 200 DUSD demo tournament, so the browser Dashboard showing a cumulative 325 DUSD during the demo is an expected difference and must not be confused with the snapshot of the first two test tournaments.

## API small-sample response measurement

After the main business flow was completed and indexed, sequential requests were made with Node fetch on the same machine, 30 per endpoint at concurrency 1, and p50/p95 were computed after sorting:

| Endpoint | p50 | p95 |
| --- | ---: | ---: |
| `/api/dashboard` | 0.497 ms | 1.494 ms |
| `/api/tournaments/1/evidence` | 1.181 ms | 1.664 ms |

This is a small-sample observation over local loopback, without isolating other host load, and cannot serve as a cloud SLA or a high-concurrency conclusion. The script's business segment of about 1,048.8 ms uses Anvil time fast-forwarding and simulated decisions, and does not represent a real-world prize processing cycle. A formal impact evaluation still requires real user task testing, a manual reconciliation baseline under the same rules, and a reasonably sufficient sample.

## Reproduction commands

```bash
(cd contracts && forge fmt --check && forge test --gas-report --gas-snapshot-emit false)
(cd contracts && forge coverage --report summary)
(cd backend && go test ./... && go vet ./...)
(cd backend && go test -race ./...)
(cd frontend && npm run lint && npm run typecheck && npm test && npm run build)
(cd backend && mkdir -p tmp && go build -o tmp/course-server ./cmd/course-server)
node tooling/smoke-course.mjs
```

Reserve disk space for the first backend compilation and frontend build. In this run the sandbox prohibited the WebSocket tests from binding local ports; after local listening was authorised, the same API tests passed; this was not skipping tests. Repeated builds reused a dedicated Go cache, which was cleared after completion to free space. Finally, a full `go test -race -p 1 -count=1 ./...` was run: all packages passed with no data races; the macOS CGO linker printed an LC_DYSYMTAB warning, but the test exit code was 0, and the raw log is kept.

## 2026-10-09 historical browser and responsive checks

Using real Chrome, the local production build `http://localhost:3001` was visited, connected to this round's separate API 18080 / Anvil 18545 (not the existing 3000/8545 services):

- The Dashboard accurately shows three tournaments, cumulative sponsorship 325, prizes paid 100, refunds 25 and funds pending settlement 200 DUSD.
- The completed tournament shows the actual 12 rounds of data; the replay slider can move to the first round; the downloaded/displayed evidence hash matches the on-chain submission.
- The audit page reads the real SponsorRefunded events, and the Refund filter works.
- At 390 px mobile width, the home page and details page have no full-page horizontal overflow, with `scrollWidth = clientWidth = 390` recorded; the audit table scrolls locally, and mobile navigation reaches the Agent login prompt.
- The final re-check fixed the status text for settled replays, long-address wrapping on mobile, and consistency between the UTF-8 length of names and the contract's 120-byte limit, and passed the final production build.
- The read-only browser checks did not perform wallet signing and cannot replace real manual multi-role MetaMask acceptance.

Screenshots: [desktop funds page](supporting-docs/evidence/dashboard-desktop.jpg), [mobile funds page](supporting-docs/evidence/dashboard-mobile.jpg), [refund audit](supporting-docs/evidence/audit-refund-desktop.jpg), [match replay](supporting-docs/evidence/tournament-replay-desktop.jpg), [mobile tournament details](supporting-docs/evidence/tournament-detail-mobile.jpg).

The first production build logged an error from WalletConnect accessing IndexedDB during SSR; this was fixed by deferring loading of the wallet Provider to the browser, and the rebuild passed. The build still reports that the optional `pino-pretty` dependency of its logging library is missing, which did not cause type, lint, page generation or this browser verification to fail.

## Items awaiting acceptance

The historical experience preparation snapshot used frontend 3001 / API 18080 / Anvil 18545, when the sponsor balance was 700 DUSD and #3 was awaiting an expiry refund; see the [raw snapshot](supporting-docs/evidence/experience-preflight-2026-10-10.json). It is not the current state: #3 has been cancelled and refunded, 033 completed the real-model and CLI funds loop, and 034 completed the #7 MetaMask main tournament; the current entry points are 3002 / 18081, and actual progress is described in the first two sections of this report and the [operating guide](supporting-docs/EXPERIENCE_GUIDE.md). Only the acceptance scope still outstanding is listed below.

- The Render Docker build, HTTPS, public URL and basic smoke test are complete; persistent disks for the free services and a cloud restore drill are not part of the scope passed this time.
- The course Sepolia broadcast, code/role reads and receipt fee records are complete; behaviour at cloud confirmation depth will be recorded by subsequent read-only acceptance.
- MetaMask cancellation-refund, dispute and other branches were not executed in 034; user signature rejection, network switching and the mobile wallet experience still lack complete acceptance evidence. The user cancelled this round's separate refund demonstration, which is not a remaining step of the 034 main tournament acceptance.
- Real Qwen calls were verified in the 033 and 034 stages; evaluation of model error rate, cost and match fairness over a reasonably sufficient sample is still to be done, and the historical deterministic simulation cannot replace this evaluation.
- Multi-user concurrency / long-running / resource exhaustion / automatic disaster recovery testing; the current design is a single-instance SQLite solution.
- Real stakeholder feedback and the final versions of the assignment report, slides and individual contributions. The final 600-second hands-on video has been completed, including the complete main business flow for tournament #13.

The standalone release repository is https://github.com/aokamutidao/agent-arena-escrow, default main; the [CI run](https://github.com/aokamutidao/agent-arena-escrow/actions/runs/38061976954) for version `9e8e429` has actually completed, with all four jobs (contracts, backend, frontend and integration) successful.
