# Prezopt Protocol

**Autonomous, ML-driven yield optimization for BlockDAG**

Prezopt is a non-custodial protocol that automatically reallocates user deposits across DeFi protocols (Aave, Compound, Curve, Yearn) to maximize risk-adjusted returns. Using machine learning to predict short-to-medium-term yield differentials and executing profitable rebalances via keeper bots, Prezopt delivers optimized yield with zero manual effort.

Built for BlockDAG from inception, Prezopt includes the $PZT token for staking, fee revenue sharing, keeper incentives, and decentralized governance.

---

## Architecture Overview

Prezopt is composed of six independently developed and deployed repositories:

| Repository | Purpose |
|-----------|---------|
| [`prezopt-contracts`](#prezopt-contracts) | Core protocol contracts: Vault, Executor, $PZT, Staking, Governance |
| [`prezopt-simulations`](#prezopt-simulations) | Simulated yield protocols and tokens for MVP on BlockDAG testnet |
| [`prezopt-ml-service`](#prezopt-ml-service) | Machine learning service that predicts profitable rebalances |
| [`prezopt-keeper`](#prezopt-keeper) | Off-chain bot that executes rebalances and claims $PZT rewards |
| [`prezopt-subgraph`](#prezopt-subgraph) | The Graph subgraph for indexing on-chain events |
| [`prezopt-frontend`](#prezopt-frontend) | User interface for deposits, withdrawals, staking, and governance |

All components are deployed on **BlockDAG testnet** for the MVP. When real protocols deploy to BlockDAG mainnet, simulated strategies will be replaced by real adapters with no changes to core logic.

---

## Repository Details

### prezopt-contracts

Core Prezopt protocol contracts deployed on BlockDAG.

- **PrezoptVault**: ERC-4626 compliant vault that accepts deposits, issues shares, and collects fees
- **RebalanceExecutor**: Executes atomic rebalances upon verified ML signals
- **$PZT Token**: Native utility and governance token
- **PZTStaking**: Enables staking for fee revenue and APY boosts
- **KeeperRewards**: Distributes $PZT to keepers for successful rebalances
- **Governance**: $PZT-based voting for protocol upgrades

Built with Foundry. Fully tested and verified.

[GitHub](https://github.com/prezopt/prezopt-contracts)

---

### prezopt-simulations

Simulated yield protocols and tokens for MVP testing on BlockDAG testnet.

- **Mock Tokens**: USDC, USDT, WETH (mintable ERC-20)
- **Mock Reward Tokens**: AAVE, COMP, CRV, YFI
- **Mock Strategies**:
  - `MockAaveStrategy`: 7.2% APY + stkAAVE emissions
  - `MockCompoundStrategy`: 8.1% APY + COMP emissions
  - `MockCurve3PoolStrategy`: 9.3% APY + CRV emissions + slippage modeling
  - `MockYearnStrategy`: 6.8% APY + YFI emissions

All strategies implement the `IStrategy` interface and are fully compatible with core contracts.

[GitHub](https://github.com/prezopt/prezopt-simulations)

---

### prezopt-ml-service

Machine learning service that predicts profitable rebalances.

- Fetches protocol state from The Graph subgraph
- Trains ensemble models (XGBoost + LSTM) on historical simulated data
- Generates cryptographically signed (EIP-712) rebalance signals
- Serves signals via REST API to keeper bot

Includes data fetchers, feature engineering, model training, and FastAPI server.

[GitHub](https://github.com/prezopt/prezopt-ml-service)

---

### prezopt-keeper

Off-chain keeper bot that executes profitable rebalances.

- Polls ML service every 60 seconds
- Simulates current gas and slippage costs
- Executes rebalances on BlockDAG via RebalanceExecutor
- Claims $PZT rewards after successful execution
- Logs all actions for monitoring

Built with Python, Web3.py, and requests.

[GitHub](https://github.com/prezopt/prezopt-keeper)

---

### prezopt-subgraph

The Graph subgraph that indexes all on-chain events.

- Indexes deposits, withdrawals, rebalances, fee distributions
- Tracks staking, governance, and $PZT rewards
- Provides real-time GraphQL API for frontend and ML service
- Deployed to The Graph Studio for BlockDAG

[GitHub](https://github.com/prezopt/prezopt-subgraph)

---

### prezopt-frontend

User interface for Prezopt Protocol.

- Connect wallet and deposit mock tokens
- View real-time allocations across 4 strategies
- Stake $PZT for fee revenue and APY boosts
- Participate in governance voting
- Monitor rebalance history and profits

Built with Next.js, wagmi, and RainbowKit. Deployed to Vercel.

[GitHub](https://github.com/prezopt/prezopt-frontend)

---

## Integration Flow

1. User deposits MockUSDC into PrezoptVault on BlockDAG
2. ML service queries The Graph for current state across 4 simulated strategies
3. ML generates signed rebalance signal if net profit > $1
4. Keeper bot validates signal and executes transaction on BlockDAG
5. RebalanceExecutor withdraws from source strategy, swaps, deposits to destination
6. Events indexed by The Graph → displayed in frontend
7. $PZT rewards distributed to stakers and keepers

---

## Deployment

All contracts deployed to **BlockDAG testnet**.  
Subgraph deployed to **The Graph Studio**.  
ML service and keeper run on **AWS ECS**.  
Frontend deployed to **Vercel**.

---

## Future Roadmap

- **Q3 2025**: Mainnet launch with real protocols as they deploy to BlockDAG
- **Q4 2025**: Multi-asset vaults, cross-chain strategies, advanced ML models
- **2026**: Institutional API, custom risk profiles, ZK-proof model verification

---

## Community

- **Website**: https://prezopt-frontend.vercel.app/  
- **Twitter**: [@prezopt](https://twitter.com/prezopt)  
- **Discord**: [Join](https://discord.gg/prezopt)  
- **GitHub**: https://github.com/prezopt  

*Autonomous yield. Community owned. Built for BlockDAG.*