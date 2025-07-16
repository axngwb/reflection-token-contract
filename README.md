# Reflection Token Contract

## Overview

This is a reflection token smart contract built on Ethereum using Solidity ^0.6.12. It implements a deflationary mechanism where token holders automatically receive rewards through reflections from transactions.

## Token Details

- **Name**: Myth
- **Symbol**: MYTH  
- **Decimals**: 8
- **Total Supply**: 10,000,000,000 (100 million * 10^8)
- **Contract Standard**: ERC-20 compatible with reflection mechanics

## Key Features

### 1. Reflection Mechanism
- Token holders automatically earn rewards from every transaction
- Reflections are distributed proportionally to all holders based on their token balance
- No staking required - simply hold tokens to earn passive income

### 2. Exclusion System
- Contract owner can exclude/include addresses from receiving reflections
- Excluded addresses don't earn reflections but can still hold and transfer tokens
- Useful for exchanges, liquidity pools, or other smart contracts

### 3. Zero Transfer Fee
- Currently configured with 0% transfer fee (`tAmount.div(100).mul(0)`)
- All transfers are fee-free, making it ideal for everyday transactions
- Fee structure can be modified by updating the `_getTValues` function

## Smart Contract Architecture

### Core Components

1. **Reflection Engine**: Manages the dual balance system (rOwned/tOwned)
2. **Exclusion System**: Handles accounts that don't participate in reflections
3. **Standard ERC-20 Functions**: Full compatibility with ERC-20 standard
4. **Ownership Controls**: Owner-only functions for managing exclusions

### Key Functions

#### Public Functions
- `reflect(uint256 tAmount)`: Manually distribute tokens to all holders
- `reflectionFromToken(uint256, bool)`: Calculate reflection value for a token amount
- `tokenFromReflection(uint256)`: Convert reflection amount back to tokens
- `isExcluded(address)`: Check if an address is excluded from reflections
- `totalFees()`: View total fees distributed as reflections

#### Owner Functions
- `excludeAccount(address)`: Exclude an address from receiving reflections
- `includeAccount(address)`: Include a previously excluded address

## Technical Implementation

### Reflection Mathematics
The contract uses a sophisticated mathematical model:
- `_rTotal`: Total reflection supply (MAX - (MAX % _tTotal))
- `_rOwned`: Reflection balances for each holder
- `_tOwned`: Token balances for excluded accounts
- Rate calculation: `_rTotal / _tTotal` determines conversion rates

### Transfer Types
The contract handles four different transfer scenarios:
1. **Standard**: Non-excluded to non-excluded
2. **To Excluded**: Non-excluded to excluded account
3. **From Excluded**: Excluded to non-excluded account  
4. **Both Excluded**: Between two excluded accounts

## Security Features

- **SafeMath Library**: Prevents overflow/underflow vulnerabilities
- **Address Validation**: Checks for zero addresses in transfers and approvals
- **Ownership Controls**: Critical functions restricted to contract owner
- **Reentrancy Protection**: Uses checks-effects-interactions pattern

## Usage Examples

### For Regular Users
```solidity
// Check your balance (includes reflections)
uint256 balance = mythToken.balanceOf(userAddress);

// Transfer tokens
mythToken.transfer(recipient, amount);

// Approve spending
mythToken.approve(spender, amount);
```

### For Contract Owner
```solidity
// Exclude an exchange from reflections
mythToken.excludeAccount(exchangeAddress);

// Include an address back in reflections
mythToken.includeAccount(userAddress);
```

## Deployment Information

- **Solidity Version**: ^0.6.12
- **License**: Unlicensed
- **Dependencies**: OpenZeppelin-style contracts (Context, IERC20, SafeMath, Address, Ownable)

## Important Notes

1. **Reflection Rewards**: Holders automatically earn from all transactions
2. **No Transfer Fees**: Currently set to 0% for all transfers
3. **Owner Privileges**: Owner can manage account exclusions
4. **Gas Optimization**: Efficient reflection calculations minimize gas costs

## Risks and Considerations

- Smart contract risk: Code should be audited before mainnet deployment
- Owner centralization: Owner has significant control over exclusions
- Reflection complexity: Users should understand how reflections work
- Gas costs: Complex calculations may result in higher gas fees

## Getting Started

1. Deploy the contract to your preferred Ethereum network
2. The deployer automatically becomes the owner and receives all initial tokens
3. Configure exclusions for exchanges, pools, or other contracts as needed
4. Token holders automatically start earning reflections from transactions

---

*This is an experimental token contract. Please conduct thorough testing and auditing before any mainnet deployment.* 