# Coding Standards

## Go Code Standards

### Basic Rules
- Go version: 1.21+
- Format all code with `go fmt`
- Check for common mistakes with `go vet`
- Line length must not exceed 100 characters

### Naming
```go
// Package names: lowercase, single word
package engine
package agent

// Exported: initial capital letter
type GameState struct { ... }
func NewGame() *Game { ... }

// Unexported: initial lowercase letter
type agentState struct { ... }
func calculateScore() int { ... }

// Interfaces: end in "er" or are named after a behaviour
type LLMClient interface { ... }
type Store interface { ... }
```

### Error Handling
```go
// Always check errors
result, err := doSomething()
if err != nil {
    return fmt.Errorf("doSomething failed: %w", err)
}

// Do not swallow errors
// ❌ result, _ := doSomething()
// ✅ result, err := doSomething(); if err != nil { ... }
```

### Project Structure
```
backend/
├── cmd/server/main.go     # Entry point
├── internal/              # Internal packages (not exported)
│   ├── engine/            # Game engine
│   ├── agent/             # AI Agent
│   ├── blockchain/        # Contract interaction
│   ├── api/               # HTTP/WS service
│   └── config/            # Configuration
└── go.mod
```

---

## Solidity Code Standards

### Basic Rules
- Solidity version: 0.8.24
- Use the Foundry framework
- Follow the Solidity Style Guide

### Naming
```solidity
// Contracts: PascalCase
contract BettingPool { ... }

// Functions: camelCase
function placeBet() external { ... }

// State variables: camelCase
uint256 public totalBetRed;

// Constants: UPPER_SNAKE_CASE
uint256 constant PROTOCOL_FEE_BPS = 500; // 5%

// Events: PascalCase
event BetPlaced(uint256 indexed gameId, address indexed user);

// Enums: PascalCase
enum GameStatus { Open, Locked, Finished }
```

### Security
- Use OpenZeppelin libraries (ReentrancyGuard, SafeERC20)
- Validate all external inputs
- Use `require` with meaningful error messages
- Watch for integer overflow (checked automatically in Solidity 0.8+)

### Gas Optimisation
- Do not store data that can be computed
- Use `calldata` rather than `memory` (for read-only parameters)
- Combine state variable reads and writes
- Mark event fields `indexed` so the frontend can filter easily

---

## TypeScript / Frontend Code Standards

### Basic Rules
- TypeScript strict mode
- Use functional components + Hooks
- Component files in PascalCase, utility files in camelCase

### Naming
```typescript
// Components: PascalCase
function BetPanel() { ... }

// Hooks: "use" prefix
function useWebSocket() { ... }

// Types: PascalCase
interface GameState { ... }
type Side = 'red' | 'blue'

// Constants: UPPER_SNAKE_CASE
const MAX_ROUNDS = 30
```

### Component Structure
```typescript
// 1. Type definitions
interface Props { gameId: number }

// 2. Component
export function BetPanel({ gameId }: Props) {
  // 3. Hooks
  const { data } = useGame(gameId)

  // 4. Computation
  const odds = useMemo(() => calculateOdds(data), [data])

  // 5. Rendering
  return <div>...</div>
}
```

---

## General Rules

### Comments
- Public APIs must have comments
- Do not write self-evident comments
- Mark pending work with TODO/FIXME

### Logging
- Use structured logging (zap or slog)
- Key operations must be logged
- Log levels: Debug / Info / Warn / Error

### Configuration
- Environment variables > configuration file > default values
- Sensitive information (private keys, API keys) only via environment variables
- Provide a `.env.example` file
