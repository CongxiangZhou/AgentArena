# Git Workflow

## Branching Strategy

```
main           ← Stable version (used for the demo)
  └── dev      ← Development branch
       └── feat/001-init       ← Feature branch
       └── feat/002-contracts  ← Feature branch
       └── fix/xxx             ← Fix branch
```

## Commit Conventions

```
<type>(<scope>): <description>

type:
  feat     - New feature
  fix      - Bug fix
  docs     - Documentation
  test     - Tests
  refactor - Refactoring
  chore    - Miscellaneous

scope:
  contract - Contracts
  engine   - Game engine
  agent    - AI Agent
  api      - Backend API
  frontend - Frontend
  devlog   - Development log

Examples:
  feat(contract): implement BettingPool
  fix(engine): fix collision detection
  docs(devlog): add iteration 001 log
```

## Commit Frequency

- Commit at least once after each iteration is complete
- Large features may be split into multiple commits
- Commit the devlog together with the code

## What Not to Commit

- `.env` files (contain private keys and other sensitive information)
- `node_modules/`
- `out/` (Foundry build output)
- `.DS_Store`
