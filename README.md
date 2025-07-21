# LiquidSwap - Next-Generation Decentralized Exchange Protocol

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Clarity](https://img.shields.io/badge/Clarity-Smart%20Contract-orange.svg)
![Stacks](https://img.shields.io/badge/Stacks-Blockchain-purple.svg)

## Overview

LiquidSwap revolutionizes DeFi trading with an advanced automated market maker (AMM) that combines deep liquidity pools, intelligent price discovery, and seamless token swapping in a trustless, permissionless environment. Built on the Stacks blockchain using Clarity smart contracts, LiquidSwap provides institutional-grade security with maximum capital efficiency.

## Core Features

### 🔄 **Advanced Trading Engine**

- **Constant Product Formula**: Optimal price stability with sophisticated AMM algorithms
- **Intelligent Slippage Protection**: Real-time risk assessment and price impact minimization
- **Zero-Custody Trading**: Complete user control with non-custodial architecture
- **Atomic Swaps**: Guaranteed transaction execution or complete rollback

### 💰 **Liquidity Management**

- **Dynamic Pool Creation**: Multi-token support with flexible pair combinations
- **LP Token Rewards**: Yield-generating liquidity provision with proportional fee sharing
- **Precision-Based Calculations**: Advanced mathematics preventing rounding exploits
- **Emergency Controls**: Pool pause/resume mechanisms for maximum fund protection

### 🛡️ **Security & Governance**

- **Administrative Controls**: Owner-restricted configuration and emergency functions
- **Comprehensive Error Handling**: Detailed error codes for robust error management
- **Gas-Optimized Architecture**: Efficient contract design minimizing transaction costs
- **Composable Design**: Seamless integration with other DeFi protocols

## System Architecture

### Contract Structure

```
LiquidSwap Protocol
├── Core Trading Engine
│   ├── AMM Pricing Algorithm
│   ├── Liquidity Pool Management
│   └── Token Swap Execution
├── Liquidity Provision System
│   ├── LP Token Minting/Burning
│   ├── Position Tracking
│   └── Reward Distribution
├── Administrative Layer
│   ├── Protocol Fee Management
│   ├── Pool Pause/Resume Controls
│   └── Owner Authorization
└── Query Interface
    ├── Pool Information
    ├── Exchange Rate Calculation
    └── Provider Position Details
```

### Data Architecture

#### Core Data Structures

**Pool Data Map**

```clarity
pools: {
  token-x: principal,      // First token contract
  token-y: principal,      // Second token contract  
  reserve-x: uint,         // Token X reserves
  reserve-y: uint,         // Token Y reserves
  total-shares: uint,      // Total LP tokens issued
  active: bool            // Pool operational status
}
```

**Liquidity Provider Tracking**

```clarity
liquidity-providers: {
  pool-id: uint,
  provider: principal
} -> {
  shares: uint            // LP token balance
}
```

**Fee Accumulation**

```clarity
accumulated-fees: principal -> uint
```

## Data Flow Architecture

### 1. Pool Creation Flow

```
User Request → Authorization Check → Token Validation → Pool Initialization → Pool ID Assignment
```

### 2. Liquidity Addition Flow

```
Token Amounts → Pool Validation → Token Transfers → LP Token Calculation → Position Update
```

### 3. Token Swap Flow

```
Swap Request → Pool Lookup → Price Calculation → Slippage Check → Token Exchange → Reserve Update
```

### 4. Liquidity Removal Flow

```
LP Tokens → Ownership Verification → Asset Calculation → Token Withdrawal → Position Cleanup
```

## Key Functions

### Public Functions

#### Pool Management

- `create-pool(token-x, token-y)` - Create new trading pair
- `add-liquidity(pool-id, token-x, token-y, amount-x, amount-y, min-shares)` - Provide liquidity
- `remove-liquidity(pool-id, token-x, token-y, shares, min-amount-x, min-amount-y)` - Withdraw liquidity

#### Trading

- `swap-exact-tokens(pool-id, token-in, token-out, amount-in, min-amount-out, x-to-y)` - Execute token swap

#### Administration

- `set-protocol-fee(new-fee)` - Configure protocol fee rate
- `pause-pool(pool-id)` - Emergency pool suspension
- `resume-pool(pool-id)` - Restore pool operations

### Read-Only Functions

- `get-pool-info(pool-id)` - Retrieve pool details
- `get-provider-shares(pool-id, provider)` - Query LP position
- `get-exchange-rate(pool-id)` - Calculate current exchange rate

## Mathematical Model

### AMM Pricing Formula

The protocol implements a sophisticated constant product formula with integrated fee structure:

```
output_amount = (input_amount * (1 - fee) * output_reserve) / 
                (input_reserve + input_amount * (1 - fee))
```

Where:

- `fee` = Protocol fee rate (default: 0.3%)
- Input/output reserves maintain the invariant: `x * y = k`

### LP Token Calculation

**Initial Liquidity**: `shares = sqrt(amount_x * amount_y)`

**Subsequent Additions**: `shares = min(amount_x * total_shares / reserve_x, amount_y * total_shares / reserve_y)`

## Security Features

### Access Controls

- **Owner Authorization**: Critical functions restricted to contract deployer
- **Input Validation**: Comprehensive parameter checking and bounds enforcement
- **Balance Verification**: Ownership and sufficiency checks before transfers

### Safety Mechanisms

- **Slippage Protection**: User-defined minimum output enforcement
- **Emergency Pause**: Immediate pool suspension capability
- **Precision Arithmetic**: 6-decimal precision preventing rounding errors
- **Atomic Operations**: All-or-nothing transaction execution

## Error Handling

| Error Code | Description |
|------------|-------------|
| `ERR-NOT-AUTHORIZED (u100)` | Unauthorized access attempt |
| `ERR-INVALID-AMOUNT (u101)` | Invalid input amount |
| `ERR-INSUFFICIENT-BALANCE (u102)` | Insufficient token balance |
| `ERR-POOL-NOT-FOUND (u103)` | Pool does not exist |
| `ERR-INVALID-POOL (u104)` | Invalid pool configuration |
| `ERR-SLIPPAGE-TOO-HIGH (u105)` | Price impact exceeds tolerance |
| `ERR-ZERO-LIQUIDITY (u106)` | Pool has no liquidity |

## Getting Started

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) for local development
- [Stacks CLI](https://github.com/hirosystems/stacks.js) for deployment
- [Node.js](https://nodejs.org/) for testing framework

### Installation

1. **Clone the repository**

```bash
git clone https://github.com/benedict-drio/liquid-swap.git
cd liquid-swap
```

2. **Install dependencies**

```bash
npm install
```

3. **Run tests**

```bash
npm test
```

4. **Check contracts**

```bash
clarinet check
```

### Development Workflow

1. **Local Testing**

```bash
clarinet console
```

2. **Deploy to Testnet**

```bash
clarinet deploy --testnet
```

3. **Integration Testing**

```bash
npm run test:integration
```

## Configuration

### Protocol Parameters

- **Default Fee Rate**: 0.3% (3000/1000000)
- **Precision**: 6 decimal places (1,000,000)
- **Minimum Liquidity**: User-defined via `min-shares`
- **Slippage Tolerance**: User-defined via `min-amount-out`

### Environment Setup

```toml
# Clarinet.toml
[contracts.liquid-swap]
path = "contracts/liquid-swap.clar"
clarity_version = 2
epoch = "2.1"
```

## Contributing

We welcome contributions to LiquidSwap! Please follow these guidelines:

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/amazing-feature`)
3. **Commit changes** (`git commit -m 'Add amazing feature'`)
4. **Push to branch** (`git push origin feature/amazing-feature`)
5. **Open a Pull Request**

### Development Standards

- Write comprehensive tests for new features
- Follow Clarity best practices and style guidelines
- Include detailed documentation for public functions
- Ensure all security checks pass

## Roadmap

### Phase 1: Core Protocol ✅

- [x] Basic AMM functionality
- [x] Liquidity provision system
- [x] Administrative controls
- [x] Security mechanisms

### Phase 2: Advanced Features 🚧

- [ ] Multi-hop routing
- [ ] Flash loan integration
- [ ] Governance token launch
- [ ] Advanced fee structures

### Phase 3: Ecosystem Integration 📋

- [ ] Cross-chain bridges
- [ ] Yield farming protocols
- [ ] Analytics dashboard
- [ ] Mobile applications

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
