# Agent Arena Escrow

A full-stack DApp for small AI tournaments: sponsors pre-fund a prize, developers register their Agents, an organiser submits match evidence, an independent auditor confirms the result, and the winning wallet claims the prize. Cancelled or expired events provide individual sponsor refunds.

This repository is the independent course release on `main`. Game mechanics and Agent decision code were reused from an earlier prototype; the tournament escrow, review and settlement domain is the course implementation. Earlier development history is retained in the original repository.

## Project links

- Repository: https://github.com/aokamutidao/agent-arena-escrow
- Frontend: https://agent-arena-course-web.onrender.com
- REST API: https://agent-arena-course-api.onrender.com
- Network: Sepolia, chain ID `11155111`
- Manager: `0x4EAFee134156c65ae64887B5e7B861977C4CFa98`; deployment block `11885235`
- [Contract deployment receipts and read verification](docs/supporting-docs/evidence/sepolia-deployment-2026-10-10.json)

Both Render services are live on release `9e8e429`. The [public smoke check](docs/supporting-docs/evidence/cloud-smoke-2026-10-10.json) passed: HTTPS page delivery, API health/readiness, Sepolia configuration and synced data. Chrome also verified language switching and the installed MetaMask connection entry. No wallet signature or business transaction was performed during this cloud check. The recorded end-to-end demonstration runs locally. All four [GitHub Actions jobs](https://github.com/aokamutidao/agent-arena-escrow/actions/runs/38061976954) passed for the same `main` release; [CI evidence](docs/supporting-docs/evidence/github-ci-2026-10-10.json) is retained.

## Implementation

| Layer | Implementation |
| --- | --- |
| Contracts | Solidity 0.8.24; Manager, Registry, Escrow and Review; 13 business transaction types |
| Backend | Go 1.25.4, Gin REST, signed nonce login, SQLite persistence, confirmed event indexing |
| Frontend | Next.js 14, TypeScript, wagmi, RainbowKit, MetaMask, bilingual responsive interface |
| Agents | Immutable owner-bound definitions; explicit LLM or labelled simulation; persisted rounds and evidence |
| Engineering | Foundry tests, fuzzing and invariants; Go and frontend tests; local integration and GitHub Actions |

Financial writes are signed in the user's wallet. The course API has no transaction-signing private key. DUSD is a six-decimal demonstration token with no redemption promise. A result hash binds evidence bytes; an independent reviewer still assesses their correctness.

## Local setup

Install Go 1.25.4, Node.js 24.11.1, Foundry 1.5.1 and a C compiler for SQLite. From a fresh checkout:

```bash
git clone https://github.com/aokamutidao/agent-arena-escrow.git
cd agent-arena-escrow
bash tooling/install-contract-deps.sh
(cd contracts && forge build && forge test --gas-snapshot-emit false)
(cd frontend && npm ci)
(cd backend && mkdir -p tmp && go build -o tmp/course-server ./cmd/course-server)
node tooling/smoke-course.mjs
```

The integration script uses a fresh local Anvil chain and temporary database, verifies funding, evidence, claims, refunds and restart behaviour, then stops its own processes. To keep a local demo running, use `node tooling/smoke-course.mjs --serve` and start the frontend with `(cd frontend && NEXT_PUBLIC_API_URL=http://localhost:8080 npm run dev)`.

For a persistent local workspace, follow [deployment and recovery](docs/DEPLOYMENT.md). Never commit deployment or model credentials.

## Documentation and evidence

Start with the [documentation index](docs/README.md), then read [requirements](docs/REQUIREMENTS.md), [architecture](docs/ARCHITECTURE.md), [contracts](docs/CONTRACT.md), [API](docs/API.md), [user guide](docs/USER_GUIDE.md) and [tests](docs/TEST_REPORT.md).

The presentation video (about 15 minutes) combines the 14-slide deck with the recorded demonstration of tournament 13: 15 real-model rounds, 30 successful calls and a 100 DUSD prize. [Script and verification](docs/supporting-docs/demo-user-recording/README.md) are versioned; the video is delivered separately.

Render free services use transient local storage and may sleep while idle. Original Agent definitions and match evidence require backups for sustained use. This course release uses SQLite; RAG, MySQL, Redis and long-term Agent memory are future work.
