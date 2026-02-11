# Owner Contract - Foundry Deployment Guide

A simple, efficient Ethereum smart contract for managing contract ownership with role-based access control. Built with [Foundry](https://getfoundry.sh/), this project includes comprehensive testing and deployment scripts.

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Building](#building)
- [Testing](#testing)
- [Deployment](#deployment)
- [Interacting with the Contract](#interacting-with-the-contract)
- [Resources](#resources)

## 🎯 Overview

The **Owner Contract** is a smart contract that:
- Sets the deployer as the initial owner
- Allows the owner to transfer ownership to a new address
- Emits events for all owner changes
- Includes owner-only access control via modifiers

**Key Features:**
- ✅ Lightweight and gas-efficient
- ✅ Event logging for transparency
- ✅ Zero-address validation
- ✅ Owner-only functions

## 📦 Prerequisites

Before deploying, ensure you have:

1. **Foundry** installed
   ```shell
   curl -L https://foundry.paradigm.xyz | bash
   foundryup
   ```

2. **Git** for cloning and version control

3. **Private key** for deployment (from a wallet like MetaMask)

4. **Native tokens** for gas fees on your target network

5. **Environment variables** configured (see [Configuration](#configuration))

## 🚀 Quick Start

### 1. Clone and Navigate
```shell
cd owner-contract
```

### 2. Install Dependencies
```shell
forge install
```

### 3. Build the Project
```shell
forge build
```

### 4. Run Tests
```shell
forge test -v
```

## ⚙️ Configuration

### Set Environment Variables

Create a `.env` file in the project root (or export to your shell):

```bash
# Private key for deployment (without 0x prefix)
PRIVATE_KEY=your_private_key_here

# RPC URL for your target network
RPC_URL=https://your-rpc-url.com

# (Optional) Etherscan API key for verification
ETHERSCAN_API_KEY=your_etherscan_key
```

### Network Examples

**Lisk Sepolia (Testnet):**
```bash
RPC_URL=https://rpc.sepolia-api.lisk.com
```

**Ethereum Mainnet:**
```bash
RPC_URL=https://eth.llamarpc.com
```

**Base Sepolia:**
```bash
RPC_URL=https://sepolia.base.org
```

**Local Anvil:**
```bash
RPC_URL=http://127.0.0.1:8545
```

## 🔨 Building

### Compile Smart Contracts
```shell
forge build
```

### Additional Build Options
```shell
# Compile with optimization
forge build --optimize

# View artifacts
ls -la out/
```

## ✅ Testing

### Run All Tests
```shell
forge test
```

### Run Tests with Verbose Output
```shell
forge test -v
```

### Run Specific Test File
```shell
forge test --match-path test/*.t.sol
```

### Generate Gas Report
```shell
forge snapshot
```

## 🌐 Deployment

### Deploy to Lisk Sepolia

#### Step 1: Set Environment Variables
```bash
export PRIVATE_KEY=your_private_key_without_0x
export RPC_URL=https://rpc.sepolia-api.lisk.com
```

#### Step 2: Deploy Using Forge Script
```shell
forge script script/Owner.s.sol:OwnerScript \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```

#### Step 3: Verify Deployment
After successful deployment, you'll see:
```
Chain 4202
Deployed to: 0x...
Transaction hash: 0x...
```

### Deploy with Verification

```shell
forge script script/Owner.s.sol:OwnerScript \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --verify \
  --verifier-url https://sepolia-blockscout.lisk.com/api \
  --etherscan-api-key $ETHERSCAN_API_KEY
```

### Deploy to Other Networks

Replace `$RPC_URL` with the appropriate network RPC endpoint:

```shell
# Ethereum Sepolia
forge script script/Owner.s.sol:OwnerScript \
  --rpc-url https://eth-sepolia.g.alchemy.com/v2/KEY \
  --private-key $PRIVATE_KEY \
  --broadcast
# Base Sepolia
forge script script/Owner.s.sol:OwnerScript \
  --rpc-url https://sepolia.base.org \
  --private-key $PRIVATE_KEY \
  --broadcast

# Local Anvil (for testing)
forge script script/Owner.s.sol:OwnerScript \
  --rpc-url http://127.0.0.1:8545 \
  --private-key $PRIVATE_KEY \
  --broadcast
```

### Dry Run (Simulate without Broadcasting)
```shell
forge script script/Owner.s.sol:OwnerScript \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY
```

## 📡 Interacting with the Contract

### Using Cast CLI

#### Get Current Owner
```shell
cast call CONTRACT_ADDRESS "owner()" \
  --rpc-url $RPC_URL
```

#### Change Owner
```shell
cast send CONTRACT_ADDRESS "changeOwner(address)" NEW_OWNER_ADDRESS \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY
```

#### Check Function Signature
```shell
cast sig "changeOwner(address)"
```

### Contract Methods

| Method | Description |
|--------|-------------|
| `owner()` | Returns the current owner address |
| `changeOwner(address newOwner)` | Transfers ownership (owner only) |
| `isOwner()` | Modifier that restricts access to owner |

## 📚 Resources

- [Foundry Book](https://book.getfoundry.sh/) - Official Foundry documentation
- [Solidity Docs](https://docs.soliditylang.org/) - Solidity language reference
- [OpenEthereum](https://docs.openethereum.network/) - Smart contract best practices
- [Lisk Sepolia Faucet](https://faucet.sepolia-api.lisk.com/) - Get testnet tokens

## 🐛 Troubleshooting

### Common Issues

**"Private key format invalid"**
- Ensure your private key doesn't include the `0x` prefix
- Private key should be 64 hex characters

**"Insufficient funds"**
- Get testnet tokens from the appropriate faucet
- Check your balance: `cast balance YOUR_ADDRESS --rpc-url $RPC_URL`

**"Contract not found"**
- Verify the contract address is correct
- Check the transaction hash on a block explorer to confirm deployment
- Ensure you're querying the correct network

**"RPC URL connection error"**
- Test RPC connectivity: `curl $RPC_URL`
- Verify the RPC URL is correct for the target network
- Check your internet connection

## 📝 License

GPL-3.0 License

---

**Happy Deploying! 🎉**
