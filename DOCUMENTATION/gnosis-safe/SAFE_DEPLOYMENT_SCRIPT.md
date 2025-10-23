# Gnosis Safe Deployment Script

This script helps you deploy a Gnosis Safe 2-of-3 multisig on Ethereum Classic Mordor testnet.

## Prerequisites

```bash
npm install ethers@6 @safe-global/protocol-kit @safe-global/api-kit
```

## Deployment Script

Save this as `deploy-safe.js`:

```javascript
const { ethers } = require('ethers');
const Safe = require('@safe-global/protocol-kit').default;
const { EthersAdapter } = require('@safe-global/protocol-kit');

const MORDOR_RPC = 'https://rpc.mordor.etccooperative.org';

async function deploySafe() {
  try {
    console.log('=== Deploying Gnosis Safe on Mordor ===\n');

    // Configuration
    const DEPLOYER_KEY = process.env.DEPLOYER_KEY;
    const OWNER_1 = process.env.OWNER_1_ADDRESS;
    const OWNER_2 = process.env.OWNER_2_ADDRESS;
    const OWNER_3 = process.env.OWNER_3_ADDRESS;
    const THRESHOLD = 2;

    if (!DEPLOYER_KEY) {
      throw new Error('DEPLOYER_KEY environment variable required');
    }
    if (!OWNER_1 || !OWNER_2 || !OWNER_3) {
      throw new Error('All three owner addresses required (OWNER_1_ADDRESS, OWNER_2_ADDRESS, OWNER_3_ADDRESS)');
    }

    // Connect to Mordor
    console.log('Connecting to Mordor testnet...');
    const provider = new ethers.JsonRpcProvider(MORDOR_RPC);
    const deployer = new ethers.Wallet(DEPLOYER_KEY, provider);
    
    console.log(`Deployer address: ${deployer.address}`);
    const balance = await provider.getBalance(deployer.address);
    console.log(`Deployer balance: ${ethers.formatEther(balance)} ETC\n`);

    if (balance < ethers.parseEther('0.1')) {
      console.warn('⚠️  Warning: Low balance. You need ETC to deploy the Safe.');
    }

    // Setup EthersAdapter
    const ethAdapter = new EthersAdapter({
      ethers,
      signerOrProvider: deployer
    });

    console.log('Configuring Safe...');
    console.log(`Owner 1: ${OWNER_1}`);
    console.log(`Owner 2: ${OWNER_2}`);
    console.log(`Owner 3: ${OWNER_3}`);
    console.log(`Threshold: ${THRESHOLD}/3 signatures required\n`);

    // Create Safe configuration
    const safeAccountConfig = {
      owners: [OWNER_1, OWNER_2, OWNER_3],
      threshold: THRESHOLD,
    };

    // Deploy Safe (this may take a few minutes)
    console.log('⏳ Deploying Safe... (this may take a few minutes)');
    console.log('Note: On some networks, Safe deployment requires multiple transactions.\n');

    const safeSdk = await Safe.create({
      ethAdapter,
      safeAccountConfig
    });

    const safeAddress = await safeSdk.getAddress();

    console.log('\n✅ Safe Deployed Successfully!\n');
    console.log('='.repeat(60));
    console.log(`Safe Address: ${safeAddress}`);
    console.log('='.repeat(60));
    console.log('\nSafe Configuration:');
    console.log(`  Owners: 3`);
    console.log(`    - ${OWNER_1}`);
    console.log(`    - ${OWNER_2}`);
    console.log(`    - ${OWNER_3}`);
    console.log(`  Threshold: ${THRESHOLD} signatures`);
    console.log('\nNext Steps:');
    console.log('  1. Verify on Blockscout:');
    console.log(`     https://etc-mordor.blockscout.com/address/${safeAddress}`);
    console.log('  2. Fund the Safe with WETC tokens');
    console.log('  3. Add SAFE_ADDRESS to GitHub Variables:');
    console.log(`     SAFE_ADDRESS=${safeAddress}`);
    console.log('  4. Test a transaction');
    console.log('\n');

  } catch (error) {
    console.error('\n❌ Deployment Failed');
    console.error(`Error: ${error.message}`);
    
    if (error.message.includes('insufficient funds')) {
      console.error('\n💡 Solution: Add more ETC to your deployer wallet');
    } else if (error.message.includes('network')) {
      console.error('\n💡 Solution: Check your internet connection and RPC endpoint');
    }
    
    process.exit(1);
  }
}

deploySafe();
```

## Usage

### Step 1: Get Owner Addresses

First, get the addresses for your three signers:

```bash
# Get address from private key
node -e "const ethers = require('ethers'); const wallet = new ethers.Wallet('YOUR_PRIVATE_KEY'); console.log(wallet.address);"
```

Run this three times for your three signer keys. Save these addresses.

### Step 2: Set Environment Variables

```bash
export DEPLOYER_KEY="0xYourDeployerPrivateKey"
export OWNER_1_ADDRESS="0xAddressFromSigner1"
export OWNER_2_ADDRESS="0xAddressFromSigner2"
export OWNER_3_ADDRESS="0xAddressFromSigner3"
```

**Note:** The deployer can be one of the signers or a separate account.

### Step 3: Deploy

```bash
node deploy-safe.js
```

### Example Output

```
=== Deploying Gnosis Safe on Mordor ===

Connecting to Mordor testnet...
Deployer address: 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb
Deployer balance: 5.234 ETC

Configuring Safe...
Owner 1: 0xABCD...1234
Owner 2: 0xEF01...5678
Owner 3: 0x9876...DCBA
Threshold: 2/3 signatures required

⏳ Deploying Safe... (this may take a few minutes)

✅ Safe Deployed Successfully!

============================================================
Safe Address: 0x1234567890ABCDEF1234567890ABCDEF12345678
============================================================

Safe Configuration:
  Owners: 3
    - 0xABCD...1234
    - 0xEF01...5678
    - 0x9876...DCBA
  Threshold: 2 signatures

Next Steps:
  1. Verify on Blockscout:
     https://etc-mordor.blockscout.com/address/0x1234...5678
  2. Fund the Safe with WETC tokens
  3. Add SAFE_ADDRESS to GitHub Variables
  4. Test a transaction
```

## Alternative: Manual Deployment

If the script doesn't work, you can deploy manually:

### Option 1: Use Safe Web App

1. Go to https://app.safe.global
2. Connect wallet to Mordor network
3. Click "Create New Safe"
4. Add three owner addresses
5. Set threshold to 2
6. Deploy

### Option 2: Use Foundry

```bash
# Clone Safe contracts
git clone https://github.com/safe-global/safe-contracts.git
cd safe-contracts

# Deploy with Foundry
forge script scripts/deploy.s.sol --rpc-url https://rpc.mordor.etccooperative.org
```

### Option 3: Direct Contract Interaction

If you're familiar with contract deployment, the Safe singleton is often already deployed on networks. You can create a Safe proxy:

```javascript
// SafeProxyFactory address (check if deployed on Mordor)
const SAFE_PROXY_FACTORY = '0x...';
const SAFE_SINGLETON = '0x...';

// Call createProxyWithNonce on the factory
```

## Verification

After deployment, verify your Safe:

```bash
# Check Safe on Blockscout
open "https://etc-mordor.blockscout.com/address/YOUR_SAFE_ADDRESS"

# Verify owners with ethers
node -e "
const ethers = require('ethers');
const provider = new ethers.JsonRpcProvider('https://rpc.mordor.etccooperative.org');
const SAFE_ABI = ['function getOwners() view returns (address[])'];
const safe = new ethers.Contract('YOUR_SAFE_ADDRESS', SAFE_ABI, provider);
safe.getOwners().then(console.log);
"
```

## Troubleshooting

### "Safe already deployed at this address"

This might happen if using a deterministic deployer. It's actually fine - you can use that Safe address!

### "Network not supported"

Gnosis Safe might not have official support for Mordor. In that case:
1. Use the manual deployment options
2. Or deploy Safe contracts yourself from their GitHub repo

### "Insufficient funds for deployment"

Safe deployment requires gas. Ensure you have:
- At least 0.1 ETC for deployment
- More if network is congested

### Safe contracts not available on Mordor

If Safe contracts aren't deployed on Mordor:
1. Deploy Safe contracts from source
2. Or use a different multisig solution compatible with ETC

## Testing Your Safe

After deployment, test it:

```bash
# 1. Send some WETC to the Safe
# Use the wrap workflow or direct transfer

# 2. Propose a test transaction
node test-safe-transaction.js

# 3. Verify on Blockscout
```

## Next Steps

Once deployed:

1. ✅ Save Safe address securely
2. ✅ Add to GitHub Variables: `SAFE_ADDRESS`
3. ✅ Fund with WETC tokens
4. ✅ Test a transaction
5. ✅ Set up monitoring
6. ✅ Configure GitHub Actions workflow

## Security Notes

- 🔒 Keep all private keys secure
- 🔒 Never commit keys to Git
- 🔒 Test thoroughly before production use
- 🔒 Consider using hardware wallets for owners
- 🔒 Keep Signer 3 offline as backup

## Resources

- [Safe Contracts GitHub](https://github.com/safe-global/safe-contracts)
- [Safe SDK Docs](https://docs.safe.global/safe-core-aa-sdk/protocol-kit)
- [ETC Mordor RPC](https://rpc.mordor.etccooperative.org)
- [Blockscout Explorer](https://etc-mordor.blockscout.com)
