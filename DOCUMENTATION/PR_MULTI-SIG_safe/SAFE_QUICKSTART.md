# 🚀 Gnosis Safe Quick Start

Get your multisig token distribution system running in 30 minutes!

## ⚡ Prerequisites Checklist

Before starting, ensure you have:

- [ ] Three Ethereum wallet private keys (for 2-of-3 Safe)
- [ ] ETC on Mordor testnet (for deployment, ~0.1 ETC)
- [ ] WETC tokens to distribute
- [ ] GitHub repository for workflows
- [ ] Node.js installed (v18+)

---

## 🎯 Quick Setup (30 Minutes)

### Step 1: Generate Addresses (5 min)

Get the addresses for your three signers:

```bash
# Install ethers if needed
npm install ethers@6

# Get address from each private key
node -e "const {Wallet} = require('ethers'); console.log('Signer 1:', new Wallet('YOUR_KEY_1').address);"
node -e "const {Wallet} = require('ethers'); console.log('Signer 2:', new Wallet('YOUR_KEY_2').address);"
node -e "const {Wallet} = require('ethers'); console.log('Signer 3:', new Wallet('YOUR_KEY_3').address);"
```

Save these three addresses! 📝

---

### Step 2: Deploy Safe (10 min)

**Option A: Using the deployment script**

```bash
# Set environment variables
export DEPLOYER_KEY="0xYourKey"
export OWNER_1_ADDRESS="0xSigner1Address"
export OWNER_2_ADDRESS="0xSigner2Address"
export OWNER_3_ADDRESS="0xSigner3Address"

# Install dependencies
npm install ethers@6 @safe-global/protocol-kit @safe-global/api-kit

# Run deployment script (see SAFE_DEPLOYMENT_SCRIPT.md)
node deploy-safe.js
```

**Option B: Manual (if script doesn't work)**

See `SAFE_DEPLOYMENT_SCRIPT.md` for alternatives.

**Result:** Save your Safe address! 
```
SAFE_ADDRESS=0xYourSafeAddressHere
```

---

### Step 3: Fund the Safe (2 min)

Send WETC to your Safe:

```bash
# Via existing wrap workflow
# Or direct transfer to Safe address

# Verify on Blockscout
open "https://etc-mordor.blockscout.com/address/$SAFE_ADDRESS"
```

---

### Step 4: Configure GitHub (5 min)

#### A. Add Secrets

Go to: `Settings → Secrets and variables → Actions → Secrets`

Add:
```
Name: SIGNER_1_KEY
Value: 0xYourSigner1PrivateKey

Name: SIGNER_2_KEY
Value: 0xYourSigner2PrivateKey
```

#### B. Add Variables

Go to: `Settings → Secrets and variables → Actions → Variables`

Add:
```
Name: SAFE_ADDRESS
Value: 0xYourSafeAddress

Name: TOKEN_AMOUNT
Value: 1.0

Name: WETC_ADDRESS (optional)
Value: 0x1953cab0E5bFa6D4a9BaD6E05fD46C1CC6527a5a
```

---

### Step 5: Deploy Workflows (5 min)

```bash
# Copy workflow to your repo
cp .github/workflows/safe-token-distribution.yml YOUR_REPO/.github/workflows/

# Copy PR template
cp .github/PULL_REQUEST_TEMPLATE.md YOUR_REPO/.github/

# Commit and push
git add .github/workflows/safe-token-distribution.yml
git add .github/PULL_REQUEST_TEMPLATE.md
git commit -m "Add Safe multisig token distribution"
git push
```

---

### Step 6: Test! (3 min)

1. **Create test PR** with your own wallet address:
   ```
   Title: Token Request: 0xYourTestAddress
   Body: Address: 0xYourTestAddress
   ```

2. **Watch the magic:**
   - ✅ Bot validates (immediate)
   - ✅ Signer 1 proposes to Safe (< 30 sec)
   - ✅ Comment posted with status

3. **Approve the PR:**
   - Click "Review changes" → "Approve"
   - ✅ Signer 2 executes (< 1 min)
   - ✅ Tokens sent!

4. **Verify:**
   ```bash
   open "https://etc-mordor.blockscout.com/address/YOUR_TEST_ADDRESS"
   ```

---

## ✅ Success Checklist

After testing, verify:

- [ ] Safe deployed and verified on Blockscout
- [ ] Safe has WETC balance
- [ ] Three owners configured (2-of-3 threshold)
- [ ] SIGNER_1_KEY and SIGNER_2_KEY in GitHub Secrets
- [ ] SAFE_ADDRESS in GitHub Variables
- [ ] Workflow file in .github/workflows/
- [ ] PR template in .github/
- [ ] Test PR completed successfully
- [ ] Tokens received and verified
- [ ] All comments and labels working

---

## 🎓 Understanding the Flow

### When PR is Opened:

```
User submits PR
     ↓
Bot validates address (immediate)
     ↓
Signer 1 proposes to Safe (< 30 sec)
     ↓
Comment: "Transaction Proposed (1/2)"
     ↓
Status: Awaiting review
```

### When PR is Approved:

```
Maintainer approves
     ↓
Signer 2 detects approval (< 30 sec)
     ↓
Signer 2 signs transaction
     ↓
Safe executes (2/2 signatures)
     ↓
Tokens sent!
     ↓
Comment: "Transaction Executed!"
```

---

## 🔒 Security Quick Check

Verify your setup is secure:

- [ ] Signer 3 is offline (not in GitHub)
- [ ] Private keys never committed to Git
- [ ] `.gitignore` includes key patterns
- [ ] Safe requires 2 signatures
- [ ] Test amount is small initially
- [ ] Monitoring is set up
- [ ] You understand the 2-of-3 model

---

## 🛠️ Common Issues & Quick Fixes

### Issue: "Signer not an owner"
```bash
# Verify addresses match
node -e "console.log(new (require('ethers').Wallet)('KEY').address)"
# Check against Safe owners on Blockscout
```

### Issue: "Insufficient balance"
```bash
# Check Safe balance
open "https://etc-mordor.blockscout.com/address/$SAFE_ADDRESS"
# Send more WETC if needed
```

### Issue: Workflow doesn't run
```bash
# Verify file location
ls .github/workflows/safe-token-distribution.yml

# Check Actions are enabled
# Settings → Actions → General → Allow all actions
```

---

## 📊 Configuration Options

### Change Distribution Amount

```
Variable: TOKEN_AMOUNT
Value: 2.5  # Send 2.5 WETC per request
```

### Use Different Token

```
Variable: WETC_ADDRESS
Value: 0xYourTokenAddress
```

### Adjust Safe Settings

To change threshold or owners:
1. Use Safe web interface
2. Or use Safe SDK
3. Requires current threshold signatures

---

## 🚀 Going Live

Once tested successfully:

1. **Fund Safe properly**
   - Don't over-fund (security)
   - Keep some reserves
   - Monitor balance

2. **Set distribution amounts**
   - Start conservative
   - Adjust based on demand

3. **Announce to community**
   - Share how to request
   - Link to PR template
   - Explain review process

4. **Monitor actively**
   - Check PRs daily
   - Review workflow logs
   - Watch Safe balance

---

## 📚 Next Steps

Now that you're set up:

1. **Read full documentation**
   - `SAFE_SETUP_GUIDE.md` - Complete guide
   - `SAFE_FAQ.md` - Common questions
   - `SAFE_DEPLOYMENT_SCRIPT.md` - Deployment details

2. **Set up monitoring**
   - Watch Safe balance
   - Alert on low balance
   - Monitor PR activity

3. **Plan for scaling**
   - Rate limiting
   - Multiple approvers
   - Batch distributions

4. **Security hardening**
   - Regular audits
   - Key rotation plan
   - Incident response plan

---

## 🆘 Getting Help

**Quick answers:** Check `SAFE_FAQ.md`

**Detailed guide:** Read `SAFE_SETUP_GUIDE.md`

**Deployment issues:** See `SAFE_DEPLOYMENT_SCRIPT.md`

**Workflow problems:** Check Actions logs

**Still stuck?** Open an issue with:
- What you tried
- Error messages
- Configuration (without keys!)

---

## 📈 Metrics to Track

Monitor these for success:

- **Distribution Count:** How many successful distributions
- **Safe Balance:** Current token balance
- **Average Time:** PR open → tokens sent
- **Success Rate:** Successful vs failed
- **User Satisfaction:** PR comments, feedback

---

## ⚠️ Important Reminders

- 🔴 This is for **TESTNET ONLY**
- 🔴 Never use on mainnet without security audit
- 🔴 Keep Signer 3 offline and secure
- 🔴 Test thoroughly before going live
- 🔴 Monitor constantly after launch
- 🔴 Have incident response plan

---

## 🎉 You're Ready!

Congratulations! Your secure multisig token distribution system is live.

**Timeline:**
- ✅ Deploy Safe: 10 min
- ✅ Configure GitHub: 5 min
- ✅ Deploy workflows: 5 min
- ✅ Test: 3 min
- ✅ Go live: Now!

**What users see:**
1. Open PR with wallet
2. Get validated instantly
3. Wait for approval
4. Receive tokens automatically

**What you do:**
1. Review PRs (legitimacy check)
2. Approve good requests
3. Monitor system health

Simple, secure, automated! 🚀

---

**Ready to start?** Begin with Step 1 above! 👆
