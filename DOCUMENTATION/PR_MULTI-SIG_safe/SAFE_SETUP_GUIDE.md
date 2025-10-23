# 🔐 Gnosis Safe Multisig Token Distribution

Complete guide for setting up PR-based token distribution using a Gnosis Safe 2-of-3 multisig wallet as treasury.

## 🎯 Overview

**Security Model:**
- 🏦 Gnosis Safe 2-of-3 multisig acts as treasury
- 🔑 Two signers stored in GitHub Secrets
- 🔑 One signer held externally (offline/backup)
- ✍️ Signer 1: Proposes transaction when PR is submitted
- ✍️ Signer 2: Approves and executes when PR is reviewed
- 🛡️ Requires 2 signatures to execute any transaction

**Workflow:**
```
PR Submitted → Signer 1 Proposes TX → Review → Signer 2 Executes → Tokens Sent
```

---

## 📋 Prerequisites

### 1. Three Ethereum Wallets

You need three separate wallets for a 2-of-3 Safe:

**Signer 1 (GitHub Secret):**
- Purpose: Proposes transactions automatically
- Storage: GitHub Secrets as `SIGNER_1_KEY`
- Usage: High frequency (every PR)

**Signer 2 (GitHub Secret):**
- Purpose: Approves and executes transactions
- Storage: GitHub Secrets as `SIGNER_2_KEY`
- Usage: High frequency (every approval)

**Signer 3 (Offline):**
- Purpose: Emergency recovery, manual overrides
- Storage: Hardware wallet, secure location (NOT in GitHub)
- Usage: Rare (emergency only)

### 2. Deploy Gnosis Safe

#### Option A: Use Safe Web Interface
1. Go to https://app.safe.global (if available on Mordor)
2. Connect to Mordor network
3. Create new Safe
4. Add 3 owner addresses
5. Set threshold to 2

#### Option B: Deploy Manually

```bash
# Clone Safe contracts
git clone https://github.com/safe-global/safe-contracts.git
cd safe-contracts

# Deploy to Mordor
# Follow their deployment guide for custom networks
```

#### Option C: Use Safe SDK

See `SAFE_DEPLOYMENT_SCRIPT.md` for automated deployment script.

---

## 🔧 Setup Instructions

### Step 1: Deploy Your Safe

Deploy a 2-of-3 Safe on Mordor testnet with:
- Owner 1: Address from SIGNER_1_KEY
- Owner 2: Address from SIGNER_2_KEY  
- Owner 3: Your offline wallet address
- Threshold: 2 signatures required

**Note your Safe address:** `0x...`

### Step 2: Fund the Safe

Send WETC tokens to your Safe address:
```bash
# Get your Safe address
SAFE_ADDRESS=0xYourSafeAddressHere

# Send WETC to the Safe (use wrap workflow or transfer directly)
```

Verify on Blockscout:
```
https://etc-mordor.blockscout.com/address/YOUR_SAFE_ADDRESS
```

### Step 3: Configure GitHub Repository

#### A. Add Secrets (Settings → Secrets and variables → Actions → Secrets)

**Required Secrets:**
```
Name: SIGNER_1_KEY
Value: 0xYourSigner1PrivateKey

Name: SIGNER_2_KEY
Value: 0xYourSigner2PrivateKey
```

⚠️ **Security:** Never commit these keys or share them!

#### B. Add Variables (Settings → Secrets and variables → Actions → Variables)

**Required Variables:**
```
Name: SAFE_ADDRESS
Value: 0xYourSafeAddress

Name: TOKEN_AMOUNT
Value: 1.0

Name: WETC_ADDRESS (optional)
Value: 0x1953cab0E5bFa6D4a9BaD6E05fD46C1CC6527a5a
```

### Step 4: Copy Workflow Files

```bash
# Copy the Safe workflow
cp .github/workflows/safe-token-distribution.yml YOUR_REPO/.github/workflows/

# Copy the PR template
cp .github/PULL_REQUEST_TEMPLATE.md YOUR_REPO/.github/
```

### Step 5: Test the System

1. **Create test PR** with your own wallet address
2. **Verify** Signer 1 proposes transaction
3. **Approve** the PR
4. **Verify** Signer 2 executes transaction
5. **Check** tokens received on Blockscout

---

## 📊 How It Works

### Stage 1: PR Submitted (Automatic)

```
User submits PR with wallet address
         ↓
Bot validates address format
         ↓
Signer 1 proposes Safe transaction
         ↓
Comment posted: "Transaction Proposed (1/2 signatures)"
         ↓
Status: Awaiting review
```

**What happens:**
- GitHub Action validates the wallet address
- Connects to Safe using Signer 1
- Creates Safe transaction for token transfer
- Signs with Signer 1 (first signature)
- Transaction is proposed but NOT executed yet
- PR labeled: `safe-proposed`, `pending-review`

### Stage 2: PR Approved (Automatic)

```
Maintainer approves PR
         ↓
Signer 2 adds second signature
         ↓
Transaction executes automatically
         ↓
Tokens sent to recipient
         ↓
Comment posted: "Transaction Executed!"
```

**What happens:**
- GitHub Action detects PR approval
- Connects to Safe using Signer 2
- Recreates and signs the same transaction
- Transaction now has 2/2 required signatures
- Safe executes the transaction automatically
- Tokens transferred to recipient
- PR labeled: `safe-executed`, `tokens-sent`

---

## 🔒 Security Benefits

### Why Use a Safe?

✅ **No Single Point of Failure**
- Requires 2 keys to execute
- One compromised key ≠ loss of funds

✅ **Transparent Operations**
- All transactions visible on-chain
- Complete audit trail

✅ **Emergency Recovery**
- Third signer (offline) can intervene
- Can remove/replace compromised signers

✅ **Automated Yet Secure**
- Automation proposals with Signer 1
- Human approval needed for execution
- Two-step verification process

### Attack Scenarios & Protections

**Scenario 1: Signer 1 Key Compromised**
- ❌ Cannot execute transactions alone
- ✅ Can only propose transactions
- ✅ Requires Signer 2 to execute

**Scenario 2: Signer 2 Key Compromised**
- ❌ Cannot propose transactions
- ❌ Cannot execute without existing proposal
- ✅ Limited damage

**Scenario 3: Both Signers Compromised**
- ⚠️ Can execute transactions
- ✅ Signer 3 (offline) can remove them
- ✅ All txs visible on-chain for monitoring

**Scenario 4: Automated Spam PRs**
- ✅ Signer 1 proposes, but doesn't execute
- ✅ Human review required via PR approval
- ✅ No automated execution without approval

---

## ⚙️ Configuration Options

### Adjust Token Amount

```
Variable: TOKEN_AMOUNT
Value: 2.5
```

### Use Different Token

```
Variable: WETC_ADDRESS
Value: 0xYourTokenAddress
```

### Change Safe Address

```
Variable: SAFE_ADDRESS
Value: 0xYourNewSafeAddress
```

---

## 🧪 Testing Checklist

Before going live:

- [ ] Safe deployed on Mordor
- [ ] Safe has 3 owners configured
- [ ] Threshold set to 2
- [ ] Safe funded with WETC tokens
- [ ] SIGNER_1_KEY added to GitHub Secrets
- [ ] SIGNER_2_KEY added to GitHub Secrets
- [ ] SAFE_ADDRESS added to GitHub Variables
- [ ] TOKEN_AMOUNT configured
- [ ] Workflow file copied to repository
- [ ] Test PR created with own address
- [ ] Signer 1 successfully proposes
- [ ] PR approved triggers Signer 2
- [ ] Transaction executes successfully
- [ ] Tokens received and verified
- [ ] All labels applied correctly

---

## 🛠️ Troubleshooting

### "Signer 1 is not an owner of this Safe"

**Cause:** The address derived from SIGNER_1_KEY doesn't match a Safe owner

**Fix:**
1. Get address from SIGNER_1_KEY: `node -e "console.log(new (require('ethers').Wallet)('YOUR_KEY').address)"`
2. Verify it matches one of the Safe owners
3. If not, update Safe owners or use correct key

### "Insufficient Safe balance"

**Cause:** Safe doesn't have enough tokens

**Fix:**
1. Check Safe balance: `https://etc-mordor.blockscout.com/address/YOUR_SAFE`
2. Send more WETC to Safe address
3. Retry workflow

### "Transaction already executed"

**Cause:** Duplicate execution attempt

**Fix:**
- This is working correctly (preventing double-send)
- Close the PR

### "No contract found at Safe address"

**Cause:** SAFE_ADDRESS is incorrect or not deployed

**Fix:**
1. Verify Safe address in Variables
2. Check if Safe is deployed: `https://etc-mordor.blockscout.com/address/YOUR_SAFE`
3. Deploy Safe if needed

### Workflow doesn't trigger

**Cause:** Workflow permissions or file location

**Fix:**
1. Ensure file is in `.github/workflows/`
2. Check Actions are enabled
3. Verify workflow syntax with: `yamllint safe-token-distribution.yml`

---

## 📈 Advanced Configuration

### Rate Limiting by User

Add this check before proposing:

```yaml
- name: Check User Rate Limit
  run: |
    # Check if user requested in last 7 days
    # Fail if they did
```

### Notification System

Add Discord/Slack notifications:

```yaml
- name: Notify Discord
  env:
    WEBHOOK: ${{ secrets.DISCORD_WEBHOOK }}
  run: |
    curl -X POST "$WEBHOOK" -d '{"content":"Safe TX proposed"}'
```

### Manual Override with Signer 3

If GitHub is compromised:
1. Connect MetaMask to Mordor
2. Go to Safe web interface
3. Use Signer 3 to reject pending transactions
4. Use Signer 3 to change Safe owners

### Monitoring

Set up monitoring for:
- Safe balance (alert when low)
- Pending transactions (alert on unusual activity)
- Failed workflows (immediate notification)

---

## 🔄 Safe Management

### Add New Owner

```javascript
// Use Safe SDK or web interface
await safeSdk.createAddOwnerTx(newOwnerAddress, threshold);
```

### Remove Compromised Owner

```javascript
// Requires current threshold signatures
await safeSdk.createRemoveOwnerTx(compromisedAddress, threshold);
```

### Change Threshold

```javascript
// Increase security: 2 → 3
await safeSdk.createChangeThresholdTx(3);
```

### Batch Operations

For efficiency, you can batch multiple distributions:

```javascript
const transactions = recipients.map(addr => ({
  to: wetcAddress,
  value: '0',
  data: encodeTransfer(addr, amount)
}));

await safeSdk.createTransaction({ transactions });
```

---

## 📚 Resources

### Gnosis Safe Documentation
- [Safe Docs](https://docs.safe.global/)
- [Protocol Kit](https://docs.safe.global/safe-core-aa-sdk/protocol-kit)
- [Safe Contracts](https://github.com/safe-global/safe-contracts)

### Ethereum Classic Mordor
- [Mordor Testnet](https://github.com/etclabscore/mordor)
- [Blockscout](https://etc-mordor.blockscout.com/)
- [WETC Token](https://etc-mordor.blockscout.com/token/0x1953cab0E5bFa6D4a9BaD6E05fD46C1CC6527a5a)

### Security Best Practices
- [Safe Security Practices](https://help.safe.global/en/articles/40797-safe-security-practices)
- [Multisig Best Practices](https://blog.openzeppelin.com/gnosis-safe-multisig-best-practices)

---

## ⚠️ Important Warnings

- 🔴 **Never** commit private keys to Git
- 🔴 **Never** share private keys publicly
- 🔴 **Always** test on testnet first
- 🔴 **Keep** Signer 3 offline and secure
- 🔴 **Monitor** Safe balance regularly
- 🔴 **Audit** transactions periodically
- 🔴 This is for **TESTNET ONLY** - do not use on mainnet without proper security audit

---

## 🎯 Next Steps

1. ✅ Read this entire guide
2. ✅ Deploy your Safe
3. ✅ Configure GitHub secrets/variables
4. ✅ Test with small amount
5. ✅ Set up monitoring
6. ✅ Announce to community
7. ✅ Start accepting requests!

**Questions?** See `SAFE_FAQ.md` for common questions and answers.
