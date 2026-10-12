# Local Real-LLM Demo: Learning and Operation

The goal is to create a new tournament on the existing course chain, actually run the model, and then verify prize settlement and an independent cancellation refund. **The real end-to-end loop was completed on 2026-10-10: tournament #5 used `qwen3.5-plus` to complete 14 rounds with 28 successful calls, and the Blue side claimed 100 DUSD; tournament #6 was cancelled and 25 DUSD was refunded.** Actual receipts, balances, usage and limitations are in the [test report](../TEST_REPORT.md). You can currently open `http://localhost:3002/tournaments/5` directly to view the record without connecting a wallet.

All commands are run from the repository root. First understand three boundaries: the Go API reads the chain and stores Agents, matches and evidence; fund transactions are signed by wallets; on-chain hashes bind off-chain content but cannot prove that the result itself is genuine or correct.

## 1. Keep the Old Environment, Prepare a New Entry Point

| Item | Address or location | Purpose |
| --- | --- | --- |
| Original demo | API `18080` / frontend `3001` | Keeps existing tournaments and data |
| New demo | API `http://localhost:18081` / frontend `http://localhost:3002` | Started; 033 CLI business acceptance passed; full MetaMask flow still under acceptance; new configuration and independent SQLite |
| Shared local chain | `http://127.0.0.1:18545`, chain ID `31337` | Course contracts deployed; DUSD is a test token with 6 decimals |
| New data directory | `.local/llm-demo/` | Database, preparation records, subsequent business evidence |

`tooling/prepare-local-demo.py` has already been used to take an online backup of the original SQLite to `.local/llm-demo/course.db`, preserving committed WAL data, and to generate `preparation.json`, `anvil-before.json` and the root `.env.course`. Below is a reproduction template; first replace the path with the real old database. Since a backup already exists, there is no need to re-run it; the script refuses to overwrite an existing configuration or target database.

```bash
python3 tooling/prepare-local-demo.py --source-db "/absolute/path/to/source-course.db"
```

Both APIs share the same Anvil chain. New transactions also enter the old API's index and statistics; this is not a fully chain-isolated environment. Do not restart the old `smoke-course.mjs --serve`, reset the chain or automatically restore `anvil-before.json` in order to prepare a new tournament.

## 2. Configuration Pre-check and Single Probe

The operator fills in `LLM_API_KEY`, `LLM_PROVIDER`, `LLM_MODEL` and `LLM_ENDPOINT` in `.env.course` to match their model service permissions, and keeps `SIMULATION_ENABLED=false` for the real demo. The endpoint should be the full address of a compatible chat API; set a per-call timeout and a per-match call budget. The combination currently verified to work is model `qwen3.5-plus` with request parameter `enable_thinking:false`; for the official match, the actual request parameters must also be checked. Credentials stay only on the server and do not go into `NEXT_PUBLIC_*`, Git, videos or evidence JSON.

After the configuration is loaded, first run the pre-check, which **does not call the model**:

```bash
set -a
source .env.course
set +a
(cd backend && go run ./cmd/llm-check)
```

The output should clearly show `request_sent=false` and report only the redacted configuration. `configured=true` only indicates that a configuration exists and cannot replace a successful model response.

Run the following probe within the authorised scope of model calls; each run sends one request and may incur costs. Read the redacted diagnostic result first before making targeted configuration adjustments:

```bash
(cd backend && go run ./cmd/llm-check --call)
```

A probe `status=succeeded` still does not mean a complete match succeeded. The actual diagnostic sequence: the first attempt returned HTTP 400 without retaining the error body; the second confirmed that `qwen3-coder-plus` is not supported, and the third confirmed that `qwen3-coder-next` is not supported; the fourth switched to `qwen3.5-plus` with `enable_thinking:false` and succeeded, with HTTP 200, `WAIT`, 43 tokens and 1468 ms. The real tournament acceptance described above was then completed separately, with the metadata of every model call archived together with the rounds.

Start the new API / frontend according to [Running and Deployment](../DEPLOYMENT.md); `./start.sh` reads `PORT=18081` and `FRONTEND_PORT=3002` from `.env.course`. Do not start it again if it is already running. Check `/ready` and `/api/config`: the chain ID, Manager and child contracts must match, the index must have caught up with the chain, and `llm_configured=true`, `execution_mode=llm`. These two LLM fields are still only configuration status and cannot replace the probe or the match record.

## 3. Staged Acceptance of a New Tournament

`tooling/local-course-flow.mjs` only allows a loopback API/RPC, Anvil and chain ID `31337`. It uses the public Anvil development accounts, does not read user wallet keys, does not deploy contracts, grant roles or mint tokens; the organiser, auditor and sponsor must already have the roles and test balances.

First view the commands and run the offline self-check, then prepare the non-fund data and pre-transaction balances:

```bash
node tooling/local-course-flow.mjs --help
node tooling/local-course-flow.mjs --self-check
node tooling/local-course-flow.mjs agents
node tooling/local-course-flow.mjs status > .local/llm-demo/preflight-balances.json
```

`agents` signs in and saves the Agent strategies bound to the two wallets, without sending on-chain transactions; the strategies require legal actions and engaging as soon as possible, without specifying a winner. `status` is completely read-only; the redirect saves the balances at the time of execution, and they can only be called an initial snapshot if collected before any new transactions.

After confirming that the probe succeeded, run in order:

```bash
node tooling/local-course-flow.mjs prepare
node tooling/local-course-flow.mjs run
node tooling/local-course-flow.mjs settle
node tooling/local-course-flow.mjs cancel
```

| Stage | Actual actions and checks |
| --- | --- |
| `prepare` | Creates a brand-new tournament for this run, sponsors 100 DUSD, Red registers → withdraws → re-registers, Blue registers; does not call the model. If the model configuration is missing, it stops before creation or funding. |
| `run` | Waits for the registration deadline, the organiser starts the tournament and requests one real LLM match; polls the existing match and does not re-request a match that is running or completed. |
| `settle` | Verifies `mode=llm`, the call statistics, the successful call records for both sides in every round, and the complete evidence hash; submits the result with the actual winner, waits for the full review window, the designated auditor approves, and then the winner claims. |
| `cancel` | Creates a separate Open tournament, sponsors 25 DUSD, saves a cancellation reason hash, the organiser cancels, and the sponsor claims a refund of the original amount. No model is run for this tournament. |

The registration deadline, final settlement deadline and review window are all calculated on **chain time**, defaulting to 180 seconds and 7200 seconds after creation and 60 seconds after submission respectively. Actual match duration depends on the model, and completion within ten minutes cannot be guaranteed. Normal commands wait for chain time to advance; if the chain stops producing blocks, the wait stops once it reaches its budget.

When the local demo needs to compress waiting, you can explicitly add `--allow-time-warp` to `run` / `settle`. The script records the target, reason and before/after times of `evm_increaseTime` / `evm_mine`; it refuses to fast-forward if other non-terminal tournaments not belonging to this run exist. Fast-forwarding affects the shared chain, and the compressed time must not be treated as real business processing time.

## 4. Interruption, Refunds and Evidence

Always reuse `.local/llm-demo/business-flow.json`; do not delete the checkpoint and re-run the fund stages. The file stores this run's tournament IDs, transaction hashes, gas, before/after balances, model call statistics and time adjustments. The script only writes to tournaments newly created in this run; when resuming after saving a transaction intent it checks the original hash, and does not automatically send a new transaction because of a re-run.

A failed model request or parse pauses the match, without fabricating actions, rounds or a winner. First check with `status`; only after confirming that additional model calls are allowed should you use `run --resume-llm`, and the budget still accumulates historical requests. Old `llm` labels without call statistics cannot pass the real-call acceptance.

A real draw does not submit a fictitious winner; `settle` waits for expiry and then refunds. If the main tournament fails or is abandoned, you can use:

```bash
node tooling/local-course-flow.mjs cancel --main-timeout
```

It waits for the applicable deadline, then triggers expiry cancellation and sponsor refunds; a tournament that is already Finalized cannot take this refund path. Only when both the receipt and the actual balance difference are verified can you write "prize claimed / refunded". The checkpoint does not store the model key, JWT, full provider request prompts or raw response bodies; the call statistics are backend observation records, not provider signatures or an independent audit.

## 5. Demo Differences Between CLI and MetaMask

The CLI above is an acceptance of real local transaction signatures and receipts from public test accounts, and **cannot be claimed as completed MetaMask UI operation**. A browser demo additionally requires connecting the corresponding test account and the `18545 / 31337` network on `3002`, signing in, and then operating according to your identity in that tournament; check the amount, operation and network in the transaction confirmation dialog before signing in the wallet. When switching between organiser, participant, auditor or sponsor, the corresponding account must also be used.

The local web pages and API can anonymously show public tournaments, fund ledgers and existing matches; write transactions require a wallet, and "My Agents" management and match runs require the corresponding signed-in identity. Agent definitions can be queried publicly and should not contain secrets. A public cloud page will not make other people's browsers automatically access your `localhost`; a public deployment needs an accessible API/RPC and a new testnet deployment, and you cannot directly reuse local ports or local accounts to claim Sepolia acceptance.
