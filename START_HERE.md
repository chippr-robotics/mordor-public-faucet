# 🚀 Getting Started - Choose Your Path

Welcome! This repository provides **three powerful systems** for Ethereum Classic Mordor testnet:

## 📦 What's Included?

### 1️⃣ Manual Wrap/Unwrap System
Convert your ETC ↔ WETC on demand with a button click

### 2️⃣ PR-Based Token Distribution - Single Key
Users request tokens via Pull Requests, you approve, tokens sent automatically

### 3️⃣ PR-Based Token Distribution - Safe{Core} Catacomb ⭐ NEW!
Secure 2-of-3 multisig treasury with file-based requests on official ETC platform

---

## ⚡ Quick Decision Guide

### Q1: Who will use this system?

- **Just me** → Use Manual Wrap/Unwrap
- **Small group (< 10 users)** → Use PR Distribution Single-Key
- **Community/treasury (> 10 users)** → Use PR Distribution Catacomb ⭐

### Q2: What's your security requirement?

- **Basic (personal use)** → Manual Wrap/Unwrap
- **Moderate (small project)** → PR Distribution Single-Key
- **High (treasury/serious)** → PR Distribution Catacomb ⭐

### Q3: How much are you distributing?

- **Just testing** → Any system works
- **< 100 tokens** → Single-Key OK
- **> 100 tokens** → Use Catacomb Multisig ⭐

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
   - Settings → Secrets → Actions
   - Name: `PRIVATE_KEY`
   - Value: Your private key

3. **Use it!**
   - Go to Actions tab
   - Select "Wrap ETC" or "Unwrap WETC"
   - Click "Run workflow"

**📖 Full Guide:** [QUICKSTART.md](QUICKSTART.md)

---

## 🎯 Path 2: PR Distribution - Single Key

**Perfect for:** Small community projects, study groups, workshops

### Files Needed:
```
.github/workflows/token-distribution-pr.yml
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
   - Settings → Variables → Actions
   - Name: `TOKEN_AMOUNT` (e.g., "1.0")

4. **Fund your wallet**
   - Send WETC to your wallet on Mordor

5. **Users can now request tokens!**
   - They open PRs with their addresses
   - You approve → tokens sent automatically

**📖 Full Guide:** [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md)

---

## 🔒 Path 3: PR Distribution - Safe{Core} Catacomb ⭐ RECOMMENDED

**Perfect for:** Treasuries, long-term projects, serious distributions

### What is Catacomb?

- **Platform:** [multisig.etccooperative.org](https://multisig.etccooperative.org/)
- **Technology:** Safe{Core} (industry-standard multisig)
- **Network:** Ethereum Classic (Mordor testnet)
- **Security:** 2-of-3 multisig (no single point of failure)

### Files Needed:
```
catacomb/.github/workflows/catacomb-safe-distribution.yml
catacomb/.github/PULL_REQUEST_TEMPLATE.md
catacomb/requests/  (directory for user request files)
```

### 30-Minute Setup:

1. **Deploy Safe{Core} on Catacomb**
   - Visit [multisig.etccooperative.org](https://multisig.etccooperative.org/)
   - Create 2-of-3 Safe with three owner addresses
   - Copy Safe address

2. **Create requests directory**
   ```bash
   mkdir -p requests
   echo "# Request files go here" > requests/.gitkeep
   ```

3. **Copy workflow files**
   ```bash
   cp catacomb/.github/workflows/catacomb-safe-distribution.yml YOUR_REPO/.github/workflows/
   cp catacomb/.github/PULL_REQUEST_TEMPLATE.md YOUR_REPO/.github/
   ```

4. **Configure GitHub**
   - **Secrets:**
     - `SIGNER_1_KEY` - First signer private key
     - `SIGNER_2_KEY` - Second signer private key
   - **Variables:**
     - `SAFE_ADDRESS` - Your Safe address
     - `TOKEN_AMOUNT` - Amount per request

5. **Fund your Safe**
   - Send WETC to Safe address

6. **Test and go live!**

**📖 Quick Start:** [catacomb/CATACOMB_QUICKSTART.md](catacomb/CATACOMB_QUICKSTART.md)  
**📖 Full Guide:** [catacomb/CATACOMB_SETUP_GUIDE.md](catacomb/CATACOMB_SETUP_GUIDE.md)

### Why Choose Catacomb?

✅ **No single point of failure** - Requires 2 of 3 keys  
✅ **Emergency recovery** - Third key offline for backup  
✅ **File-based requests** - No PR conflicts!  
✅ **Official platform** - ETC Cooperative's Catacomb  
✅ **Industry standard** - Safe{Core} battle-tested security  
✅ **Professional** - Best practices for treasury management

---

## 🔄 Path 4: All Systems

You can use multiple systems! They work independently.

**Example setup:**
- **Manual wrap/unwrap** for your own token management
- **PR Single-Key** for quick community requests
- **PR Catacomb** for treasury and serious distributions

Just copy all workflow files you need!

---

## 📊 Feature Comparison

| Feature | Manual | PR Single-Key | PR Catacomb ⭐ |
|---------|--------|---------------|----------------|
| **Setup Time** | 5 min | 15 min | 30 min |
| **Security** | 🔒 Basic | 🔒🔒 Moderate | 🔒🔒🔒 High |
| **Keys Required** | 1 | 1 | 3 (2 active) |
| **Automation** | Manual | Automated | Automated |
| **Recovery** | None | None | ✅ Yes |
| **Best For** | Personal | Small projects | Treasuries |
| **PR Conflicts** | N/A | ❌ Possible | ✅ None (files) |
| **Platform** | - | - | ✅ Official ETC |

---

## 📚 Documentation Map

### Overview
- 👉 **This file** - Choose your path
- [INDEX.md](INDEX.md) - Complete navigation
- [SYSTEM_OVERVIEW.md](SYSTEM_OVERVIEW.md) - Compare all systems

### Manual Wrap/Unwrap
- [QUICKSTART.md](QUICKSTART.md) - 5-minute setup
- [README.md](README.md) - Complete documentation

### PR Single-Key Distribution
- [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md) - Complete guide
- [PR_QUICK_REFERENCE.md](PR_QUICK_REFERENCE.md) - Quick reference
- [WORKFLOW_DIAGRAM.md](WORKFLOW_DIAGRAM.md) - Visual flows

### PR Catacomb Multisig ⭐
- [catacomb/README.md](catacomb/README.md) - Overview
- [catacomb/CATACOMB_QUICKSTART.md](catacomb/CATACOMB_QUICKSTART.md) - 30-min setup
- [catacomb/CATACOMB_SETUP_GUIDE.md](catacomb/CATACOMB_SETUP_GUIDE.md) - Complete guide
- [catacomb/MIGRATION_GUIDE.md](catacomb/MIGRATION_GUIDE.md) - Upgrade from old system

---

## ⚙️ Common Setup Steps

### 1. GitHub Secrets Setup

**Where:** Repository → Settings → Secrets and variables → Actions

**For Manual & Single-Key:**
- Name: `PRIVATE_KEY`
- Value: Your private key

**For Catacomb:**
- Name: `SIGNER_1_KEY` (first signer)
- Name: `SIGNER_2_KEY` (second signer)

⚠️ **NEVER commit private keys to your repository!**

### 2. Get Test ETC

**Option A:** Use a Mordor faucet
- Google "Mordor ETC faucet"
- Request test ETC for your address

**Option B:** Ask in ETC communities
- Discord, Telegram, Reddit
- Request test ETC

**Option C:** Use existing ETC
- If you already have Mordor ETC

### 3. Wrap ETC to WETC (if needed)

**For PR Distribution:**
```bash
# Use the wrap workflow
# Or use WETC directly if you have it
```

---

## 🎯 Recommended Path by Use Case

### Personal Testing/Development
→ **Path 1: Manual Wrap/Unwrap**  
Simple, fast, direct control

### Study Group (< 10 students)
→ **Path 2: PR Single-Key**  
Quick setup, good enough security

### Hackathon (50+ participants)
→ **Path 3: PR Catacomb** ⭐  
Secure, professional, scalable

### Open Source Project
→ **Path 3: PR Catacomb** ⭐  
Best security, file-based (no conflicts)

### University Course (ongoing)
→ **Path 3: PR Catacomb** ⭐  
Long-term, secure, institutional-grade

### Quick Demo
→ **Path 1: Manual**  
Fastest to set up

---

## 🚦 Traffic Light System

🟢 **Green Light (Use Immediately)**
- Manual wrap/unwrap for personal use
- PR single-key for small, time-limited projects

🟡 **Yellow Light (Consider Carefully)**
- PR single-key for long-term projects
- PR single-key for large amounts

🔴 **Red Light (Use Catacomb Instead)**
- Any treasury > 100 tokens
- Long-term projects (> 1 semester)
- Institutional use

---

## ⚡ Ultra-Quick Start

**30 seconds to decide:**

1. Just testing personally? → Manual
2. Small group, quick project? → Single-Key
3. Treasury, serious project? → Catacomb ⭐
4. Not sure? → Start with Manual, upgrade later

**Choose now and jump to your path above!**

---

## 🆘 Need Help?

### Before You Start
- Read the appropriate guide for your chosen path
- Make sure you understand the security model
- Test with small amounts first

### During Setup
- Check the troubleshooting sections
- Review GitHub Actions logs
- Verify secrets and variables are set correctly

### After Launch
- Monitor the first few requests closely
- Be ready to help users with issues
- Keep Safe/wallet funded

### Still Stuck?
- Check [INDEX.md](INDEX.md) for all documentation
- Review workflow logs in Actions tab
- Open an issue with details

---

## 🎉 Ready to Start?

**Pick your path above and get building!**

- ⚡ **Fast** → Manual (5 min)
- 🎯 **Simple** → Single-Key (15 min)
- 🔒 **Secure** → Catacomb (30 min) ⭐

**All systems are production-ready for Mordor testnet!**

---

**Questions?** Start with [INDEX.md](INDEX.md) for complete navigation!

**Confused?** Read [SYSTEM_OVERVIEW.md](SYSTEM_OVERVIEW.md) for detailed comparison!