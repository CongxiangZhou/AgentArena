# Running, Deployment and Recovery

This document contains reproducible operating instructions. For the steps that were actually executed, see the [test report](TEST_REPORT.md); Docker, cloud services, testnet URLs and real-wallet demos cannot be considered verified on the strength of configuration files alone.

## Current Public Configuration

Published repository: https://github.com/aokamutidao/agent-arena-escrow , default branch `main`.

| Setting | Value |
| --- | --- |
| Frontend | https://agent-arena-course-web.onrender.com |
| API | https://agent-arena-course-api.onrender.com |
| Chain ID | 11155111 (Sepolia) |
| Manager | `0x4EAFee134156c65ae64887B5e7B861977C4CFa98` |
| Manager deployment block | 11885235 |
| DUSD | `0x36600325144a43e00A7664E7e7C0d4dDa54955f4` |

The [deployment receipts and read-only verification of the five contracts](supporting-docs/evidence/sepolia-deployment-2026-10-10.json) have been saved. Both the frontend and backend Render services are Live, running main `9e8e429`. The [public smoke record](supporting-docs/evidence/cloud-smoke-2026-10-10.json) confirms that the HTTPS home page and the API endpoints /health, /ready and /api/config return 200, ready=true, and that the Sepolia and five-contract configuration is correct. The Chrome home page, the Chinese/English language toggle, Sepolia Synced data and the Installed MetaMask connection entry all passed; no wallet signatures or business transactions were made in this check. No cloud minting, tournaments, sponsorship, prize claims or refunds were executed. The cloud deployment uses the labelled deterministic simulation; the full real-model demo was completed locally.

Both Render services use this repository's `main`, with the root directory as the Docker context. The frontend build argument `NEXT_PUBLIC_API_URL` points to the API in the table above, and `NEXT_PUBLIC_SEPOLIA_RPC_URL` is an accessible Sepolia RPC. The API's `AUTH_DOMAIN=agent-arena-course-web.onrender.com` and `CORS_ORIGINS=https://agent-arena-course-web.onrender.com`, and the chain configuration is filled in as per the table above. The JWT secret is configured only through a private server-side environment variable; the deployment wallet's private key is not uploaded.

## Current Delivery Choice (2026-10-10)

The user confirmed that completing the local real-LLM battle and the fund demo for a new tournament takes priority; both the cloud frontend and backend may use Render free Web Services. Free services sleep after 15 minutes of inactivity, and waking usually takes about one minute; a restart, sleep or redeployment loses the local SQLite file. The user accepts this demo limitation, and the video and report must state it truthfully. The official recording is based primarily on the local services; the public configuration is shown in the table above, and external acceptance checks are archived according to the actual check results.

The local SQLite database, chain state and battle evidence must be archived. On-chain events can be backfilled, but Agent definitions, battle replays and review justifications cannot be recovered from on-chain hashes alone. After the public database is reset, existing registrations may no longer find their corresponding Agent definitions; prepare the demo materials again or restore a backup before continuing.

The Render deployment uses two services, using `deploy/Dockerfile.frontend` and `deploy/Dockerfile.backend` respectively, both with the repository root as the build context. The frontend is configured at build time with the backend's public HTTPS `NEXT_PUBLIC_API_URL`; the backend's `AUTH_DOMAIN`/`CORS_ORIGINS` correspond to the frontend domain. The empty API address in `compose.yaml` relies on a same-origin Caddy reverse proxy and cannot be used as is for two separate Render domains. The free cloud environment does not run a local Anvil; an accessible testnet RPC and the new course contracts are still required.

Source: [Render free services documentation](https://render.com/docs/free) (consulted 2026-10-10). The persistent Compose template later in this document is retained for future learning about long-term deployment.

## 1. Environment and First Build

- Go 1.25.4, Node.js 24.11.1 / npm, Foundry 1.5.1, Solidity 0.8.24.
- SQLite uses `go-sqlite3`, which requires CGO and a system C compiler.
- Reserve several GB of disk space for the first install/build; there is no need to reinstall when dependencies are already present.
- All of the following commands are run from the repository root by default; changing directory inside parentheses does not change the location for the next step.

```bash
bash tooling/install-contract-deps.sh
(cd contracts && forge build)
(cd frontend && npm ci)
(cd backend && mkdir -p tmp && go build -o tmp/course-server ./cmd/course-server)
```

The contract dependency installation script pins the forge-std and OpenZeppelin commits rather than using a drifting latest. The ABI comes from this project's contracts: after modifying the contracts, run `(cd contracts && forge build && python3 export_abi.py)` and also check the generated frontend ABI.

## 2. Verify the Local End-to-End Loop with One Command

```bash
node tooling/smoke-course.mjs
```

The script automatically starts a new loopback Anvil, deploys the test token and course contracts, assigns distinct wallet roles, and then runs integrated HTTP and wallet-transaction tests. It does not connect to any existing RPC, nor does it read the deployment private key from the project `.env`.

Verification scope: single-use nonce sign-in, replay protection, rejection of unauthorised runs, real-token sponsorship/registration/match start, simulated battle evidence hashing, independent review/prize or draw refund, independent dispute refund, event-based fund reconciliation and backend restart recovery. Fixture results in the dispute scenario are clearly labelled and do not masquerade as actual AI matches.

Successful results are saved to `.artifacts/course-smoke.json`, including the gasUsed of each transaction. The temporary chain, database and test signing identities are used only for local verification; when the script exits it cleans up the processes and temporary directories it created.

## 3. Keep the Demo Running and Open the Frontend

```bash
node tooling/smoke-course.mjs --serve
```

After verification completes, it keeps the RPC `http://127.0.0.1:8545` and API `http://localhost:8080` running, prints each wallet address, and creates an open tournament that can be viewed. In another terminal, run:

```bash
(cd frontend && NEXT_PUBLIC_API_URL=http://localhost:8080 npm run dev)
```

Open `http://localhost:3000`. The local chain ID is **31337**, and the DUSD token has **6 decimal places**. `--serve` uses the public Anvil test mnemonic and account indices:

| Index | Local demo identity |
| --- | --- |
| 0 | Administrator, test token issuer |
| 1 | Organiser |
| 2 | Designated auditor |
| 3 / 4 | The two participants |
| 5 | Sponsor |

The standard Anvil test mnemonic is `test test test test test test test test test test test junk`. Import it only into a separate test wallet profile, and never deposit real assets into these public identities. After adding the local network to MetaMask, you can demonstrate each operation with the test accounts; the account addresses are as actually printed by the script.

The script advances local chain time to verify deadlines and review windows; when the demo is kept running, it actively mines one local block per second so that the registration/review deadlines continue to advance. If you continue the demo manually, the on-chain time and state are authoritative. `Ctrl+C` stops the API/Anvil created by the script and ends this temporary demo; it does not terminate other services. If the fixed ports are already in use, the script refuses to start. You can choose other ports for your own new demo:

```bash
COURSE_SMOKE_RPC_PORT=18545 COURSE_SMOKE_API_PORT=18080 COURSE_SMOKE_UI_PORT=3001 node tooling/smoke-course.mjs --serve
(cd frontend && NEXT_PUBLIC_API_URL=http://localhost:18080 NEXT_PUBLIC_LOCAL_RPC_URL=http://127.0.0.1:18545 npm run dev -- --port 3001)
```

When using a non-default RPC, the RPC address of the corresponding network in MetaMask must match as well.

## 4. Day-to-Day Persistent Development

The temporary demo above is suitable for self-checks; when you need to keep your own Agents and tournaments, use separate persistent Anvil data and SQLite.

1. Deploy `DeployCourse.s.sol` on your own local node, and note the new Manager address and the block containing the **actual deployment receipt**. The block printed during the `forge script` simulation phase cannot substitute for the actual receipt.
2. Copy `.env.course.example` to `.env.course`, generate a JWT secret with `openssl rand -hex 32`, and fill in the new deployment configuration.
3. Run `./start.sh`; by default the API is on 8080 and the frontend on 3000. The script loads only the root `.env.course`, not the old `backend/.env`.

Key variables:

| Variable | Meaning |
| --- | --- |
| `JWT_SECRET` | Random session signing key of at least 32 characters; must not be the example or an old hard-coded value |
| `DATABASE_PATH` | Resolved from the backend working directory; defaults to `course.db`; must not point to the old `arena.db` |
| `AUTH_DOMAIN` | Actual frontend host, may include the port; locally `localhost:3000` |
| `CORS_ORIGINS` | Exact allowed origins, comma-separated; must not be `*` |
| `TRUSTED_PROXY_CIDRS` | Optional, comma-separated IPs/CIDRs of controlled reverse proxies; empty by default, so forged forwarding headers are not trusted |
| `CHAIN_ID` | Local 31337 / Sepolia 11155111 |
| `RPC_URL` | Read-only RPC; no account private key required |
| `TOURNAMENT_MANAGER_ADDRESS` | New course Manager; sub-contracts/token are read and verified by the backend |
| `DEPLOYMENT_BLOCK` | Block of the Manager's actual deployment receipt, used for full backfill |
| `CONFIRMATIONS` | May be 0 locally; testnet example is 6, choose according to risk |
| `CONTRACT_ABI_DIR` | Defaults to `../contracts/abi` |
| `SIMULATION_ENABLED` | Whether the clearly labelled deterministic simulation is allowed when no model is configured |
| `LLM_API_KEY` | Server-side model credential; set `SIMULATION_ENABLED=false` for real-model demos; the old `backend/.env` is not read automatically |
| `LLM_PROVIDER` / `LLM_MODEL` | Provider label in call records / actual requested model name |
| `LLM_ENDPOINT` | Full compatible endpoint URL configured on the server, including `/chat/completions`; not returned via the API or set by participants |
| `LLM_TIMEOUT_SECONDS` / `LLM_MAX_CALLS` | Per-request timeout / per-match call budget; subsequent retries still count towards the budget |
| `NEXT_PUBLIC_API_URL` | API base URL accessed by the browser; may be an empty string with a same-origin reverse proxy |

`.env.course.example` pre-fills the local RPC but leaves the Manager empty. You must fill in the Manager, or clear both the RPC and the Manager to enter the undeployed view-only mode; filling in only one of them causes startup to be refused. When undeployed, `/health` is available, `/ready` returns 503, and fund operations are unavailable.

A manual build and start does not automatically load any `.env`; configuration must be exported explicitly. Do not inject `PRIVATE_KEY`, `OWNER_PRIVATE_KEY` or `COURSE_DEPLOYER_KEY` into the course API container. The JWT secret and model key must also never go into `NEXT_PUBLIC_*`.

## 5. Testnet Contract Deployment

The script `contracts/script/DeployCourse.s.sol` uses an explicit deployment wallet; without `--broadcast` it only simulates. The deployment environment is kept separate from the backend environment:

- `COURSE_DEPLOYER_KEY`: testnet deployment wallet, provided through local secret management.
- `COURSE_ADMIN`: must be the address of that deployment wallet.
- `COURSE_ORGANIZER` / `COURSE_AUDITOR`: different wallet addresses.
- `COURSE_TOKEN`: may be omitted/zero address, in which case the script creates a new DUSD with owner-only mint; compatibility must be assessed separately before using any other token.

After configuring these variables and double-checking the target testnet, run:

```bash
(cd contracts && forge script script/DeployCourse.s.sol:DeployCourse --rpc-url "$RPC_URL")
# Review the simulation result; after confirming the deployment identity, chain ID, fees and roles, the deployment operator runs:
(cd contracts && forge script script/DeployCourse.s.sol:DeployCourse --rpc-url "$RPC_URL" --broadcast)
```

Save the deployment transactions, chain ID, all contract addresses, the actual deployment block, and the compiler and optimiser settings. The test token owner can mint DUSD to demo sponsors; this test asset must not be described as real USDC. Before sponsoring, the spender of the ERC20 `approve` must be **PrizeEscrow**, not the Manager.

The current course Sepolia addresses are those in the deployment record at the top of this document. The existing deployment was completed using the RPC's actual gas estimates; when redeploying, the current chain estimates and fees must be checked, rather than directly reusing the gas from the local Forge simulation.

## 6. Public Cloud Template

The repository provides `compose.yaml`, `deploy/Dockerfile.backend`, `deploy/Dockerfile.frontend` and `deploy/Caddyfile`. The design is a single-instance API + SQLite persistent volume + Next.js standalone + Caddy HTTPS. The host requires Docker Compose, a domain whose DNS points to it, and inbound 80/443.

On the server, create a separate `.env.course` that is ignored by Git, and fill in `SITE_DOMAIN`, the JWT, the chain configuration and the actual deployment block; the cloud environment's chain ID and RPC must match. Pre-check and start with the following commands:

```bash
docker compose --env-file .env.course config --quiet
docker compose --env-file .env.course up -d --build
```

The frontend is built with an empty API URL, so the browser uses the same-origin `/api`, and Caddy routes it to the Go service. Only the proxy's ports are exposed by the containers, and SQLite lives in the named volume `course_data`. In the cloud, `TRUSTED_PROXY_CIDRS` should be explicitly set to the actual Caddy container IP or the CIDR of a separate controlled Docker network (verified via `docker network inspect`); do not set it to trust the entire network. Otherwise the backend will not use the forwarded client IP, and users behind the proxy will share the same per-minute write-request rate-limit bucket. Leave it empty for direct local access. Do not simply scale the API to multiple replicas: the current task scheduling and SQLite are single-instance by design.

During acceptance, check: the public HTTPS URL, `/health`, `/ready`, the correct domain in the wallet sign-in message, account/network switching, all role flows, data persistence after a container restart, rejected-signature/network-loss messages, mobile width, and RPC confirmation latency. Save actual screenshots and check times, not just the Dockerfile.

All four CI jobs (contracts, backend, frontend and Anvil integration) for this repository's main `9e8e429` have succeeded: [Actions run](https://github.com/aokamutidao/agent-arena-escrow/actions/runs/38061976954), [CI evidence](supporting-docs/evidence/github-ci-2026-10-10.json).

## 7. Backup and Failure Recovery

On-chain logs can be rebuilt, but Agent definitions, battle records and review justifications cannot be restored from hashes. Back up the entire SQLite database regularly, and verify restoration.

Locally, you can use SQLite's online backup interface:

```bash
sqlite3 backend/course.db ".backup 'backend/course.backup.db'"
```

For cloud deployments, perform an equivalent backup from an operations environment that has the SQLite tools, or stop the API and consistently back up the entire data volume; do not copy only the main `.db` file while the service is running and miss the WAL. After restoring to a new path, start the service with the **same deployment identity**, and check `/ready`, the evidence hashes, the fund totals and the event counts. Changing the JWT secret invalidates old sign-ins.

- **RPC temporarily failing**: `/ready` fails; keep the database, and indexing continues once the RPC is fixed.
- **Chain checkpoint changed**: pause operations and check for a reorg or a wrongly switched node. Back up first; stop the API; on a backup copy of the same deployment, clear `financial_ledger`, `chain_events`, `tournaments`, `index_cursors` and `index_checkpoints`, then backfill again. Keep `agent_profiles`, `matches`, `review_notes` and `database_identity`. Re-verify whether the off-chain matches still correspond to the current on-chain registrations/results; old evidence must not automatically be treated as new fact.
- **Changing chain or redeploying**: create a new database path; do not bypass the deployment identity check.
- **Battle interrupted**: the organiser requests a run again, and the last saved round is resumed; completed matches cannot be re-run and overwritten.
- **Auditor unreachable/role revoked**: after the settlement deadline, any wallet can trigger expire, and sponsors call claimRefund themselves.

The course version does not yet provide multi-region backup, automatic failover, KMS or full monitoring and alerting; the report must define these boundaries truthfully.
