# Dynamic Portfolio Optimizer Protocol

## Overview

A decentralized asset management system enabling automated portfolio rebalancing with institutional-grade precision. Implements self-custodial portfolio management through smart contract automation on the Stacks blockchain.

## Key Features

- Multi-Asset Portfolio Management
- Algorithmic Rebalancing Engine
- Non-Custodial Architecture
- Granular Allocation Controls (0.01% precision)
- Time-Based Rebalancing Triggers
- Protocol-Level Fee System
- Multi-Layer Security Model

## Technical Specifications

### Core Components

1. **Portfolio Structure**

   - Unique portfolio IDs with ownership tracking
   - Timestamped creation/rebalancing events
   - Active/inactive status flags
   - Value tracking in native units

2. **Asset Allocation Model**

   - 10-asset capacity per portfolio
   - Dual allocation tracking:
     - Target percentages (basis points)
     - Current token amounts
   - Cross-verification of token contracts

3. **Governance Parameters**
   - Protocol owner privileges
   - 0.25% management fee (25 basis points)
   - Max 20 portfolios per user

### Data Architecture

```clarity
;; Portfolio Master Record
{
    owner: principal,
    created-at: uint,
    last-rebalanced: uint,
    total-value: uint,
    active: bool,
    token-count: uint
}

;; Asset Allocation Schema
{
    target-percentage: uint,  // Basis points (0.01%)
    current-amount: uint,
    token-address: principal
}
```

## Core Functions

### Portfolio Management

1. **create-portfolio**

   - Initializes new portfolio with token allocations
   - Validates percentage sums (100% ≡ 10,000bps)
   - Enforces 10-asset maximum

2. **rebalance-portfolio**

   - Execution engine for allocation adjustments
   - Time-based trigger (24h cooldown)
   - Fee calculation module

3. **update-portfolio-allocation**
   - Dynamic percentage adjustment
   - Real-time validation checks
   - Historical tracking

### Read Operations

1. **get-portfolio**

   - Full metadata retrieval
   - Ownership verification

2. **get-portfolio-asset**

   - Granular asset position reporting
   - Cross-references token contracts

3. **calculate-rebalance-amounts**
   - Simulation engine
   - Drift detection system

## Error Handling System

| Code                           | Description                 | Resolution Path                  |
| ------------------------------ | --------------------------- | -------------------------------- |
| ERR-NOT-AUTHORIZED (100)       | Unauthorized access attempt | Verify portfolio ownership       |
| ERR-INVALID-PORTFOLIO (101)    | Nonexistent portfolio ID    | Validate portfolio existence     |
| ERR-INSUFFICIENT-BALANCE (102) | Funding shortfall           | Check token balances             |
| ERR-REBALANCE-FAILED (104)     | Allocation mismatch         | Review target percentages        |
| ERR-MAX-TOKENS-EXCEEDED (107)  | Asset overflow              | Limit to 10 assets per portfolio |

## Installation & Deployment

### Prerequisites

- Clarinet SDK v1.5.0+
- Node.js 18.x LTS
- Stacks Testnet access

### Deployment Steps

1. Clone repository

```bash
git clone https://github.com/your-org/portfolio-manager.git
cd portfolio-manager
```

2. Install dependencies

```bash
npm install @stacks/transactions @stacks/network clarinet-sdk
```

## Usage Examples

### Portfolio Creation

```clarity
(create-portfolio
    (list 'SP3FBR2AGK5H9QBDH3EEN6DF8EK8JY7RX8QJ5SVTE.token-a 'SP466FNC0P7JWTNM2R9T199QRZN1MYEDTAR0KP27.token-b)
    (list u3000 u7000)  ;; 30%/70% allocation
)
```

### Automated Rebalancing

```clarity
(rebalance-portfolio u12345)
```

### Allocation Update

```clarity
(update-portfolio-allocation u12345 u1 u6500)  ;; Update token 1 to 65%
```

## Security Model

### Access Controls

- Owner-restricted critical functions
- Principal-based authorization
- Time-locked rebalancing

### Validation Layers

1. Percentage sanity checks (0 ≤ x ≤ 10,000bps)
2. Token contract verification
3. Portfolio capacity limits
4. Input length matching

## Contribution Guidelines

1. Fork repository
2. Create feature branch
3. Submit PR with:
   - Test coverage
   - Documentation updates
   - Clarinet check output
