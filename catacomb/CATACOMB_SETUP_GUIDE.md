# 🔐 Safe{Core} Catacomb Token Distribution

Complete guide for setting up PR-based token distribution using Safe{Core} on Ethereum Classic Catacomb.

## 🎯 What is Catacomb?

**Catacomb** is the Safe{Core} deployment on **Ethereum Classic** (both mainnet and Mordor testnet).

- **Platform:** [multisig.etccooperative.org](https://multisig.etccooperative.org/)
- **Technology:** Safe{Core} (formerly Gnosis Safe)
- **Network:** Ethereum Classic & Mordor
- **Security:** Industry-standard multisig wallet

## 🆕 Key Improvements

### 1. File-Based Request System
Users create a file in `requests/` folder instead of editing PR description.

**Benefits:**
- ✅ No PR conflicts - each user modifies different file
- ✅ Easy validation - read from filesystem
- ✅ Clear audit trail - Git history per user
- ✅ Prevents blocking other PRs

### 2. Early Exit on Invalid Requests
Workflow fails fast if no valid address found.

**Benefits:**
- ✅ Other PRs aren't blocked
- ✅ Clear error messages
- ✅ Fast feedback to users

### 3. Safe{Core} Integration
Uses official Safe{Core} branding and Catacomb platform.

**Benefits:**
- ✅ Official ETC Cooperative platform
- ✅ Up-to-date with Safe ecosystem
- ✅ Better documentation and support

---

## 📋 Prerequisites

### 1. Three Ethereum Wallets

**Signer 1 (GitHub Secret):**
- Proposes transactions when PR is submitted
- Stored in: `SIGNER_1_KEY`

**Signer 2 (GitHub Secret):**
- Executes transactions when PR is approved
- Stored in: `SIGNER_2_KEY`

**Signer 3 (Offline):**
- Emergency recovery and manual operations
- NOT stored in GitHub (hardware wallet recommended)

### 2. Deploy Safe{Core} on Catacomb

#### Using the Web Interface

1. **Visit:** [multisig.etccooperative.org](https://multisig.etccooperative.org/)

2. **Connect Wallet:**
   - Click "Connect wallet"
   - Choose MetaMask or WalletConnect
   - Ensure you're on Mordor network (Chain ID: 63)

3. **Create New Safe:**
   - Click "Create new Safe Account"
   - Add three owner addresses:
     - Owner 1: Address from SIGNER_1_KEY
     - Owner 2: Address from SIGNER_2_KEY
     - Owner 3: Your offline wallet address
   - Set threshold: **2** signatures required
   - Deploy (costs minimal gas on Mordor)

4. **Note Your Safe Address:**
   ```
   0xYourSafeAddressHere
   ```

---

## 🔧 Setup Instructions

### Step 1: Create Repository Structure

```bash
# Create the requests directory
mkdir -p requests

# Add gitkeep
cat > requests/.gitkeep << 'EOF'
# This directory contains token request files
# Each file should be named: username.txt
# Content: One wallet address per file
EOF

# Create example (optional)
echo "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb" > requests/example.txt
```

### Step 2: Copy Workflow Files

```bash
# Copy workflow
cp catacomb-safe-distribution.yml YOUR_REPO/.github/workflows/

# Copy PR template
cp PULL_REQUEST_TEMPLATE.md YOUR_REPO/.github/

# Commit
git add .github/workflows/catacomb-safe-distribution.yml
git add .github/PULL_REQUEST_TEMPLATE.md
git add requests/
git commit -m "Add Safe{Core} Catacomb token distribution"
git push
```

### Step 3: Fund Your Safe

Send WETC tokens to your Safe address:

```bash
# Via wrap workflow, or direct transfer
# Verify on Blockscout:
https://etc-mordor.blockscout.com/address/YOUR_SAFE_ADDRESS
```

### Step 4: Configure GitHub

#### A. Add Secrets

Go to: `Settings → Secrets and variables → Actions → Secrets`

```
Name: SIGNER_1_KEY
Value: 0xYourSigner1PrivateKey

Name: SIGNER_2_KEY
Value: 0xYourSigner2PrivateKey
```

#### B. Add Variables

Go to: `Settings → Secrets and variables → Actions → Variables`

```
Name: SAFE_ADDRESS
Value: 0xYourSafeAddress

Name: TOKEN_AMOUNT
Value: 1.0

Name: WETC_ADDRESS (optional)
Value: 0x1953cab0E5bFa6D4a9BaD6E05fD46C1CC6527a5a
```

### Step 5: Test the System

1. **Create test branch:**
   ```bash
   git checkout -b test-request
   ```

2. **Create request file:**
   ```bash
   echo "0xYourTestAddress" > requests/yourusername.txt
   git add requests/yourusername.txt
   git commit -m "Test token request"
   git push -u origin test-request
   ```

3. **Open PR and verify:**
   - Bot validates address (< 10 sec)
   - Signer 1 proposes to Safe (< 30 sec)
   - Approve the PR
   - Signer 2 executes (< 1 min)
   - Tokens sent!

---

## 📊 How It Works

### User Flow

```
1. User creates branch
2. User creates requests/username.txt with wallet address
3. User opens PR
4. Bot validates file and address (immediate)
5. Signer 1 proposes to Safe (< 30 sec)
6. Status: 1/2 signatures
7. Maintainer reviews PR
8. Maintainer approves PR
9. Signer 2 executes transaction (< 1 min)
10. Status: 2/2 signatures → EXECUTED
11. Tokens sent to wallet!
```

### Why File-Based?

**Problem with old approach:**
- All users edit PR description
- Merge conflicts between PRs
- One invalid PR blocks others

**Solution with files:**
- Each user creates unique file
- No conflicts (different files)
- Invalid PRs don't block others
- Clean Git history

---

## 🔒 Security Model

### Safe{Core} Configuration

```
Safe Address: 0x...
├─ Owner 1: Signer 1 (GitHub) → Proposes
├─ Owner 2: Signer 2 (GitHub) → Executes
└─ Owner 3: Offline wallet → Recovery

Threshold: 2 of 3 signatures required
```

### Transaction Flow

**Step 1: Propose (PR Opened)**
```
User opens PR
  ↓
Validate file & address
  ↓
Signer 1 connects to Safe
  ↓
Creates transaction
  ↓
Signs with Signer 1 (1/2)
  ↓
Transaction PROPOSED (not executed)
```

**Step 2: Execute (PR Approved)**
```
Maintainer approves PR
  ↓
Signer 2 connects to Safe
  ↓
Recreates same transaction
  ↓
Signs with Signer 2 (2/2)
  ↓
Safe executes automatically
  ↓
Tokens sent!
```

### Attack Scenarios

**Scenario: Invalid address submitted**
- ✅ Validation fails immediately
- ✅ PR labeled `invalid-address`
- ✅ Workflow exits cleanly
- ✅ Other PRs not affected

**Scenario: Signer 1 compromised**
- ✅ Can only propose (not execute)
- ✅ Requires PR approval
- ✅ Signer 3 can remove if needed

**Scenario: Both GitHub signers compromised**
- ⚠️ Can propose and execute
- ✅ All transactions visible on-chain
- ✅ Signer 3 can intervene and remove them

**Scenario: Spam PRs**
- ✅ Each proposal costs gas (Safe balance)
- ✅ Human review required for execution
- ✅ Can be rate-limited

---

## ⚙️ Configuration Options

### Change Distribution Amount

```
Variable: TOKEN_AMOUNT
Value: 2.5
```

### Use Different Token

```
Variable: WETC_ADDRESS
Value: 0xYourTokenContractAddress
```

### Change Safe Address

```
Variable: SAFE_ADDRESS
Value: 0xYourNewSafeAddress
```

---

## 🛠️ Troubleshooting

### "No request file found"

**Cause:** User didn't create file in `requests/` folder

**Fix:**
- Ensure file path is `requests/username.txt`
- Check file is committed and pushed
- Verify PR includes the file

### "Invalid wallet address"

**Cause:** Address format is incorrect

**Fix:**
- Must start with `0x`
- Must be exactly 42 characters (0x + 40 hex)
- Only contains 0-9, a-f, A-F
- Example: `0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb`

### "Multiple request files"

**Cause:** User created more than one file

**Fix:**
- Keep only one file: `requests/yourusername.txt`
- Delete other files

### "Signer 1 is not an owner"

**Cause:** Address from SIGNER_1_KEY doesn't match Safe owner

**Fix:**
1. Get address from key:
   ```bash
   node -e "console.log(new (require('ethers').Wallet)('YOUR_KEY').address)"
   ```
2. Check Safe owners at: [multisig.etccooperative.org](https://multisig.etccooperative.org/)
3. Add address as owner if needed

### "Insufficient Safe balance"

**Cause:** Not enough tokens in Safe

**Fix:**
- Check balance: https://etc-mordor.blockscout.com/address/SAFE_ADDRESS
- Send more WETC to Safe
- Wait for confirmation

### "No contract found at Safe address"

**Cause:** Safe not deployed or wrong address

**Fix:**
- Verify SAFE_ADDRESS variable
- Check Safe exists on Mordor
- Deploy if needed: [multisig.etccooperative.org](https://multisig.etccooperative.org/)

---

## 📈 Advanced Features

### Rate Limiting by User

Add check before proposing:

```yaml
- name: Check User Rate Limit
  run: |
    # Count user's recent requests
    # Fail if too many in time window
```

### Automatic PR Closing

After successful execution:

```yaml
- name: Close PR
  uses: actions/github-script@v7
  with:
    script: |
      await github.rest.pulls.update({
        owner: context.repo.owner,
        repo: context.repo.repo,
        pull_number: context.payload.pull_request.number,
        state: 'closed'
      });
```

### Notification Integration

```yaml
- name: Notify Discord
  env:
    WEBHOOK: ${{ secrets.DISCORD_WEBHOOK }}
  run: |
    curl -X POST "$WEBHOOK" \
      -H "Content-Type: application/json" \
      -d '{"content":"✅ Tokens sent to user"}'
```

---

## 🔄 Migration from Old System

If you're upgrading from the old PR-description based system:

### Step 1: Update Workflow

Replace old workflow with `catacomb-safe-distribution.yml`

### Step 2: Update PR Template

Use new template that instructs users to create files

### Step 3: Create Requests Directory

```bash
mkdir -p requests
echo "# Request files go here" > requests/.gitkeep
git add requests/
git commit -m "Add requests directory"
git push
```

### Step 4: Announce Changes

Post announcement:
- New process requires file creation
- Link to new PR template
- Provide examples

---

## 📚 Resources

### Safe{Core} / Catacomb
- [Catacomb Platform](https://multisig.etccooperative.org/)
- [Safe{Core} Docs](https://docs.safe.global/)
- [Safe Protocol Kit](https://docs.safe.global/safe-core-aa-sdk/protocol-kit)

### Ethereum Classic
- [ETC Website](https://ethereumclassic.org/)
- [ETC Cooperative](https://etccooperative.org/)
- [Mordor Testnet](https://github.com/etclabscore/mordor)

### Explorers
- [Mordor Blockscout](https://etc-mordor.blockscout.com/)
- [WETC Token](https://etc-mordor.blockscout.com/token/0x1953cab0E5bFa6D4a9BaD6E05fD46C1CC6527a5a)

---

## ⚠️ Important Notes

- 🔴 **Testnet only** - For Mordor testnet use
- 🔴 **Safe{Core} branding** - Use "Safe{Core}" not "Gnosis Safe"
- 🔴 **Catacomb platform** - Official ETC deployment
- 🔴 **File-based requests** - No PR description editing
- 🔴 **One file per user** - Prevents conflicts
- 🔴 **Validation first** - Exit early on invalid input

---

## 🎯 Next Steps

1. ✅ Deploy Safe on Catacomb
2. ✅ Fund Safe with WETC
3. ✅ Configure GitHub secrets/variables
4. ✅ Create requests directory
5. ✅ Test with your own address
6. ✅ Announce to community
7. ✅ Monitor and maintain

**Questions?** See the FAQ or check workflow logs!
