# 🚀 Getting Started - Choose Your Path

Welcome! This repository provides two powerful systems for Ethereum Classic Mordor testnet:

## 📦 What's Included?

### 1️⃣ Manual Wrap/Unwrap System
Convert your ETC ↔ WETC on demand with a button click

### 2️⃣ PR-Based Token Distribution (Faucet)
Users request tokens via Pull Requests, you approve, tokens sent automatically

---

## ⚡ Quick Decision Guide

**Answer these questions:**

### Q1: Who will use this system?

- **Just me** → Use Manual Wrap/Unwrap
- **Community/students/developers** → Use PR Distribution

### Q2: What's your main goal?

- **Manage my own tokens** → Manual Wrap/Unwrap
- **Distribute tokens to others** → PR Distribution
- **Both** → Set up both systems!

### Q3: How technical are your users?

- **Very technical** → Either system works
- **Non-technical** → PR Distribution (they just open a PR)

---

## 🎯 Path 1: Manual Wrap/Unwrap

**Perfect for:** Personal wallet management, testing, development

### Files Needed:
```
.github/workflows/wrap-etc.yml
.github/workflows/unwrap-wetc.yml
```

### 5-Minute Setup:

1. **Copy files to your repo**
   ```bash
   cp .github/workflows/wrap-etc.yml YOUR_REPO/.github/workflows/
   cp .github/workflows/unwrap-wetc.yml YOUR_REPO/.github/workflows/
   ```

2. **Add your private key**
   - Go to: Settings → Secrets → Actions
   - Name: `PRIVATE_KEY`
   - Value: Your wallet's private key

3. **Run workflows**
   - Actions tab → Select workflow → Run workflow
   - Choose percentage (wrap) or amount (unwrap)

**📖 Full Guide:** [QUICKSTART.md](QUICKSTART.md)

---

## 🎁 Path 2: PR Token Distribution

**Perfect for:** Community faucets, workshops, developer programs

### Files Needed:
```
.github/workflows/token-distribution-pr.yml  (for WETC)
.github/workflows/etc-distribution-pr.yml    (for native ETC)
.github/PULL_REQUEST_TEMPLATE.md
```

### 15-Minute Setup:

1. **Copy files to your repo**
   ```bash
   cp .github/workflows/token-distribution-pr.yml YOUR_REPO/.github/workflows/
   cp .github/PULL_REQUEST_TEMPLATE.md YOUR_REPO/.github/
   ```

2. **Add your private key**
   - Settings → Secrets → Actions
   - Name: `PRIVATE_KEY`

3. **Set distribution amount** (optional)
   - Settings → Secrets → Actions → Variables tab
   - Name: `TOKEN_AMOUNT` (e.g., "2.5")

4. **Fund your distribution wallet**
   - Send WETC/ETC to your wallet on Mordor

5. **Users can now request tokens!**
   - They open PRs with their addresses
   - You approve → tokens sent automatically

**📖 Full Guide:** [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md)

---

## 🔄 Path 3: Both Systems

You can use both! They work independently.

**Use wrap/unwrap for:** Your own token management  
**Use PR distribution for:** Community token distribution

Just copy all workflow files!

---

## 📚 Documentation Map

### Start Here
- 👉 **This file** - Choose your path
- [INDEX.md](INDEX.md) - Complete navigation guide

### For Manual Wrap/Unwrap
- [QUICKSTART.md](QUICKSTART.md) - 5-minute setup
- [README.md](README.md) - Complete documentation

### For PR Distribution
- [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md) - Complete guide
- [PR_QUICK_REFERENCE.md](PR_QUICK_REFERENCE.md) - Quick reference
- [WORKFLOW_DIAGRAM.md](WORKFLOW_DIAGRAM.md) - Visual flows

### For Implementation
- [SETUP_COMMANDS.md](SETUP_COMMANDS.md) - Git commands

---

## ⚙️ Common Setup Steps (Both Systems)

### 1. GitHub Secrets Setup

**Where:** Repository → Settings → Secrets and variables → Actions

**Add Secret:**
- Name: `PRIVATE_KEY`
- Value: Your private key (without 0x prefix)
- ⚠️ **NEVER commit this to your repository!**

### 2. Get Test ETC

**Option A:** Use a Mordor faucet
**Option B:** Mine some ETC (testnet)
**Option C:** Ask in Ethereum Classic communities

### 3. Test First!

Always test with small amounts:
- Wrap 1% instead of 10%
- Send 0.1 tokens instead of 1.0
- Verify on Blockscout before scaling up

---

## 🔒 Security Checklist

Before going live:

- [ ] Private key added to GitHub Secrets (NOT in code)
- [ ] `.gitignore` includes private key patterns
- [ ] Tested with small amounts
- [ ] Confirmed transactions on Blockscout
- [ ] Set appropriate distribution amounts
- [ ] Understand this is TESTNET ONLY

---

## 🌐 Network Information

**Ethereum Classic Mordor Testnet**
- **Chain ID:** 63
- **RPC:** https://rpc.mordor.etccooperative.org
- **Explorer:** https://etc-mordor.blockscout.com
- **WETC Contract:** 0x1953cab0E5bFa6D4a9BaD6E05fD46C1CC6527a5a

---

## 💡 Examples & Use Cases

### Use Case 1: Personal DeFi Testing
**System:** Manual Wrap/Unwrap  
**Why:** Quick conversion between ETC and WETC for testing DeFi protocols

### Use Case 2: University Blockchain Course
**System:** PR Distribution  
**Why:** Students request tokens for course projects, professor approves

### Use Case 3: Hackathon
**System:** PR Distribution  
**Why:** Participants request tokens, organizers approve, automatic distribution

### Use Case 4: Open Source Project
**System:** PR Distribution  
**Why:** Contributors request testnet tokens for testing, maintainers approve

### Use Case 5: Personal Project with Community
**System:** Both  
**Why:** Wrap/unwrap for yourself, PR distribution for community testers

---

## 🆘 Need Help?

### Common Questions

**Q: Do I need both workflows?**  
A: No! Choose based on your use case above.

**Q: Can I modify the workflows?**  
A: Yes! They're fully customizable.

**Q: How do I know which amount to set?**  
A: Start small (0.1-1.0 tokens) and adjust based on usage.

**Q: What if my workflow fails?**  
A: Check the Actions logs, verify secrets, ensure wallet has funds.

**Q: Can users request multiple times?**  
A: Current setup allows one distribution per PR. Add rate limiting for more control.

---

## 📊 Comparison Table

| Feature | Manual Wrap/Unwrap | PR Distribution |
|---------|-------------------|-----------------|
| **Setup Time** | 5 minutes | 15 minutes |
| **User Interaction** | Actions tab | Pull Request |
| **Best For** | Personal use | Community distribution |
| **Approval Needed** | No | Yes (manual review) |
| **Automatic** | On-demand | On PR approval |
| **Complexity** | Low | Medium |
| **Documentation** | QUICKSTART.md | PR_DISTRIBUTION_GUIDE.md |

---

## 🎬 Next Steps

### For Manual Wrap/Unwrap:
1. Read [QUICKSTART.md](QUICKSTART.md)
2. Set up in 5 minutes
3. Start wrapping/unwrapping!

### For PR Distribution:
1. Read [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md)
2. Set up workflows and template
3. Test with your own wallet first
4. Announce to your community!

### For Complete Navigation:
- See [INDEX.md](INDEX.md) for full documentation map

---

## ✅ Success Checklist

**Before considering setup complete:**

For Manual Wrap/Unwrap:
- [ ] Workflows copied to `.github/workflows/`
- [ ] `PRIVATE_KEY` added to Secrets
- [ ] Test wrap transaction successful
- [ ] Test unwrap transaction successful
- [ ] Verified on Blockscout

For PR Distribution:
- [ ] Workflows copied to `.github/workflows/`
- [ ] PR template copied to `.github/`
- [ ] `PRIVATE_KEY` added to Secrets
- [ ] `TOKEN_AMOUNT` or `ETC_AMOUNT` configured
- [ ] Distribution wallet funded
- [ ] Test PR approved and tokens sent
- [ ] Verified on Blockscout

---

**Ready to start?** 

👉 [Manual Wrap/Unwrap Path](QUICKSTART.md)  
👉 [PR Distribution Path](PR_DISTRIBUTION_GUIDE.md)  
👉 [Complete Documentation](INDEX.md)

Let's build something awesome on Ethereum Classic! 🚀
