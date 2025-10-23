# 🚀 Catacomb Quick Start

Set up Safe{Core} token distribution on Ethereum Classic Mordor in 30 minutes!

## ⚡ What You're Building

A secure token distribution system where:
- 👥 Users create a file with their wallet address
- ✅ Bot validates automatically
- 🔐 2-of-3 multisig approves and executes
- 💰 Tokens sent automatically on PR approval

**Platform:** [multisig.etccooperative.org](https://multisig.etccooperative.org/) (Catacomb)

---

## 📋 Prerequisites (5 min)

- [ ] 3 Ethereum wallet private keys
- [ ] ETC on Mordor testnet (for Safe deployment)
- [ ] WETC tokens to distribute
- [ ] GitHub repository

---

## 🎯 Quick Setup (30 Minutes)

### Step 1: Get Wallet Addresses (5 min)

```bash
# Install ethers if needed
npm install ethers@6

# Get address from each private key
node -e "const {Wallet} = require('ethers'); console.log('Signer 1:', new Wallet('KEY1').address);"
node -e "const {Wallet} = require('ethers'); console.log('Signer 2:', new Wallet('KEY2').address);"
node -e "const {Wallet} = require('ethers'); console.log('Signer 3:', new Wallet('KEY3').address);"
```

📝 **Save these three addresses!**

---

### Step 2: Deploy Safe on Catacomb (10 min)

1. **Visit:** [multisig.etccooperative.org](https://multisig.etccooperative.org/)

2. **Connect Wallet:**
   - Click "Connect wallet"
   - Switch to Mordor network (Chain ID: 63)

3. **Create Safe:**
   - Click "Create new Safe Account"
   - Add 3 owners (your addresses from Step 1)
   - Set threshold: **2**
   - Click "Next" → "Create"
   - Confirm transaction

4. **Copy Safe Address:**
   ```
   SAFE_ADDRESS=0xYourSafeAddressHere
   ```

---

### Step 3: Fund Safe (2 min)

```bash
# Send WETC to your Safe address
# Verify on Blockscout:
open "https://etc-mordor.blockscout.com/address/$SAFE_ADDRESS"
```

---

### Step 4: Setup Repository (5 min)

```bash
# Create directory structure
mkdir -p requests
mkdir -p .github/workflows

# Create requests directory marker
cat > requests/.gitkeep << 'EOF'
# This directory contains token request files
# Each file should be named: username.txt
# Content: One wallet address per file
EOF

# Copy workflow
cp catacomb-safe-distribution.yml .github/workflows/

# Copy PR template
cp PULL_REQUEST_TEMPLATE.md .github/

# Commit
git add .github/ requests/
git commit -m "Add Catacomb token distribution"
git push
```

---

### Step 5: Configure GitHub (5 min)

#### A. Add Secrets
`Settings → Secrets and variables → Actions → Secrets`

```
SIGNER_1_KEY = 0xYourPrivateKey1
SIGNER_2_KEY = 0xYourPrivateKey2
```

#### B. Add Variables
`Settings → Secrets and variables → Actions → Variables`

```
SAFE_ADDRESS = 0xYourSafeAddress
TOKEN_AMOUNT = 1.0
```

---

### Step 6: Test! (3 min)

```bash
# Create test branch
git checkout -b test-request

# Create your request file
echo "0xYourTestAddress" > requests/yourusername.txt

# Commit and push
git add requests/yourusername.txt
git commit -m "Test: Request tokens"
git push -u origin test-request

# Open PR on GitHub
# Watch the automation!
```

**What happens:**
1. ✅ Bot validates (< 10 sec)
2. 🔐 Signer 1 proposes to Safe (< 30 sec)
3. 👤 You approve the PR
4. 🚀 Signer 2 executes (< 1 min)
5. ✨ Tokens sent!

---

## ✅ Success Checklist

After testing:

- [ ] Safe deployed and visible on Catacomb
- [ ] Safe has WETC balance
- [ ] Three owners configured (2-of-3)
- [ ] SIGNER_1_KEY in GitHub Secrets
- [ ] SIGNER_2_KEY in GitHub Secrets
- [ ] SAFE_ADDRESS in GitHub Variables
- [ ] TOKEN_AMOUNT configured
- [ ] requests/ directory exists
- [ ] Workflow in .github/workflows/
- [ ] Test PR completed successfully
- [ ] Tokens received and verified

---

## 📊 Understanding the Flow

### User's Experience

```
1. Fork repository
2. Create branch
3. Add file: requests/myusername.txt
4. Content: 0xMyWalletAddress
5. Commit and push
6. Open PR
7. Get validated (automatic)
8. Wait for approval
9. Receive tokens!
```

### Your Experience

```
1. Review PR
2. Check file is correct
3. Approve PR
4. Transaction executes automatically
5. Close/merge PR
6. Done!
```

---

## 🔐 Security Quick Check

- [ ] Signer 3 is offline (NOT in GitHub)
- [ ] Private keys never committed
- [ ] Safe requires 2 signatures
- [ ] Tested with small amount
- [ ] Understand 2-of-3 model

**Key Point:** One compromised key = funds still safe! ✅

---

## 🎓 How File-Based Requests Work

### Old Way (Problems)
```
❌ All users edit PR description
❌ Merge conflicts
❌ One invalid PR blocks others
```

### New Way (Better!)
```
✅ Each user creates unique file
✅ No conflicts
✅ Invalid requests don't block others
✅ Clean Git history
```

**Example:**
```
requests/
├── alice.txt      (0xAliceAddress)
├── bob.txt        (0xBobAddress)
└── charlie.txt    (0xCharlieAddress)
```

Each file is independent!

---

## 🛠️ Quick Fixes

### "No request file found"
```bash
# File must be in requests/ folder
echo "0xYourAddress" > requests/yourusername.txt
```

### "Invalid wallet address"
```bash
# Must be exactly this format:
# 0x + 40 hex characters
0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb
```

### "Signer not an owner"
```bash
# Verify address matches Safe owner
node -e "console.log(new (require('ethers').Wallet)('YOUR_KEY').address)"
# Check on: multisig.etccooperative.org
```

---

## 🚀 Going Live

Once tested:

1. **Announce:**
   - Share PR template
   - Explain file creation process
   - Provide example

2. **Monitor:**
   - Check PRs daily
   - Watch Safe balance
   - Review workflow logs

3. **Maintain:**
   - Refill Safe as needed
   - Approve legitimate requests
   - Close completed PRs

---

## 📈 Scaling Tips

### For 10-50 users
✅ Current setup works great

### For 50+ users
Consider:
- Rate limiting (1 request per week per user)
- Automated PR closing after execution
- Batch processing
- Multiple maintainers

---

## 🎯 What Makes This Secure?

### vs Single-Key Wallet

| Feature | Single Key | Safe{Core} 2-of-3 |
|---------|------------|-------------------|
| Keys needed | 1 | 2 of 3 |
| If key lost | 🔴 Funds gone | ✅ Still works |
| If key stolen | 🔴 Funds gone | ✅ Still safe |
| Recovery | ❌ None | ✅ Offline key |

### Security Layers

```
Layer 1: File validation (format check)
Layer 2: Human review (PR approval)
Layer 3: Multisig (2 keys required)
Layer 4: On-chain transparency
```

---

## 📚 Learn More

**Complete Guide:**
- [CATACOMB_SETUP_GUIDE.md](CATACOMB_SETUP_GUIDE.md)

**Platform:**
- [multisig.etccooperative.org](https://multisig.etccooperative.org/)

**Explorer:**
- [etc-mordor.blockscout.com](https://etc-mordor.blockscout.com/)

---

## 💡 Pro Tips

1. **Start Small:** Test with 0.1 tokens first
2. **Keep Signer 3 Offline:** Hardware wallet recommended
3. **Monitor Balance:** Set alerts for low balance
4. **Document Keys:** Securely record which key is which
5. **Review Carefully:** Always check addresses before approving

---

## ⚠️ Important Reminders

- 🔴 **Testnet only** - For Mordor testnet
- 🔴 **Keep Signer 3 offline** - Not in GitHub
- 🔴 **Test thoroughly** - Before announcing
- 🔴 **Monitor actively** - After launch

---

## 🎉 You're Ready!

**Timeline:**
- ✅ Deploy Safe: 10 min
- ✅ Configure GitHub: 5 min
- ✅ Setup repo: 5 min
- ✅ Test: 3 min
- ✅ Go live: Now!

**Result:**
- 🔐 Secure 2-of-3 multisig
- 🤖 Automated workflow
- 👥 File-based requests (no conflicts!)
- ✨ Professional distribution system

---

**Ready?** Start with Step 1! 👆

**Questions?** Check [CATACOMB_SETUP_GUIDE.md](CATACOMB_SETUP_GUIDE.md)

**Need help?** Review workflow logs in Actions tab
