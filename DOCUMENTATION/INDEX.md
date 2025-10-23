# 📖 Documentation Index

Welcome! This repository provides GitHub Actions workflows for Ethereum Classic Mordor testnet operations.

## 🎯 What Can You Do?

### 1. Wrap/Unwrap ETC ↔ WETC (Manual)
Convert between native ETC and wrapped ETC (WETC) tokens on demand.

**Workflows:**
- `wrap-etc.yml` - Wrap ETC to WETC
- `unwrap-wetc.yml` - Unwrap WETC to ETC

**Documentation:** 
- [README.md](README.md) - Main documentation
- [QUICKSTART.md](QUICKSTART.md) - 5-minute setup

**Use Cases:**
- Personal wallet management
- Testing wrapped token interactions
- DeFi protocol development

---

### 2. Token Distribution via Pull Requests (Faucet)
Allow users to request testnet tokens by opening PRs. Maintainers review and approve, triggering automatic distribution.

**Workflows:**
- `token-distribution-pr.yml` - Distribute WETC tokens
- `etc-distribution-pr.yml` - Distribute native ETC

**Documentation:**
- [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md) - Complete guide
- [PR_QUICK_REFERENCE.md](PR_QUICK_REFERENCE.md) - Quick reference card

**Use Cases:**
- Community testnet faucet
- Developer token distribution
- Controlled access to testnet funds
- Educational workshops

---

## 📚 Documentation Files

### For Getting Started

| File | Purpose | When to Read |
|------|---------|-------------|
| [README.md](README.md) | Main documentation for wrap/unwrap | First-time setup |
| [QUICKSTART.md](QUICKSTART.md) | 5-minute quick start | When you want to start fast |
| [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md) | Complete PR distribution guide | Setting up faucet system |
| [PR_QUICK_REFERENCE.md](PR_QUICK_REFERENCE.md) | Quick reference card | Daily operations |

### For Implementation

| File | Purpose | When to Read |
|------|---------|-------------|
| [SETUP_COMMANDS.md](SETUP_COMMANDS.md) | Git and CLI commands | Deploying to your repo |
| [INDEX.md](INDEX.md) | This file - navigation | When lost or starting out |

### Templates

| File | Purpose | Location |
|------|---------|----------|
| [PULL_REQUEST_TEMPLATE.md](.github/PULL_REQUEST_TEMPLATE.md) | Template for token requests | `.github/` |

---

## 🚀 Quick Start Paths

### Path A: I Want to Wrap/Unwrap My Own Tokens

```
1. Read: QUICKSTART.md
2. Add PRIVATE_KEY to GitHub Secrets
3. Copy workflows to your repo
4. Run workflows from Actions tab
```

### Path B: I Want to Create a Token Faucet

```
1. Read: PR_DISTRIBUTION_GUIDE.md
2. Add PRIVATE_KEY to GitHub Secrets
3. Set TOKEN_AMOUNT or ETC_AMOUNT variable
4. Copy PR workflows and template
5. Fund your distribution wallet
6. Users can now request tokens!
```

### Path C: I'm a User Requesting Tokens

```
1. Open a Pull Request
2. Fill in your wallet address
3. Wait for validation (automatic)
4. Wait for approval (maintainer review)
5. Receive tokens (automatic on approval)
```

---

## 📁 Repository Structure

```
etc-wetc-workflows/
├── .github/
│   ├── workflows/
│   │   ├── wrap-etc.yml                    # Manual: Wrap ETC → WETC
│   │   ├── unwrap-wetc.yml                 # Manual: Unwrap WETC → ETC
│   │   ├── token-distribution-pr.yml       # PR: WETC distribution
│   │   └── etc-distribution-pr.yml         # PR: ETC distribution
│   └── PULL_REQUEST_TEMPLATE.md            # Template for token requests
├── .gitignore                              # Security: Prevent key commits
├── README.md                               # Main documentation
├── QUICKSTART.md                           # 5-minute setup guide
├── SETUP_COMMANDS.md                       # Git/CLI commands
├── PR_DISTRIBUTION_GUIDE.md                # Complete PR system guide
├── PR_QUICK_REFERENCE.md                   # Quick reference card
└── INDEX.md                                # This file
```

---

## 🔑 Required Secrets & Variables

### Secrets (Settings → Secrets and variables → Actions → Secrets)

| Secret | Purpose | Required For |
|--------|---------|--------------|
| `PRIVATE_KEY` | Distribution wallet private key | All workflows |

### Variables (Settings → Secrets and variables → Actions → Variables)

| Variable | Purpose | Default | Required For |
|----------|---------|---------|--------------|
| `TOKEN_AMOUNT` | WETC per request | 1.0 | WETC distribution |
| `ETC_AMOUNT` | ETC per request | 0.5 | ETC distribution |

---

## 🎓 Learning Paths

### For Beginners
1. Start with [QUICKSTART.md](QUICKSTART.md)
2. Try wrapping 1% of your balance
3. Unwrap the tokens back
4. Read [README.md](README.md) for details

### For Project Maintainers
1. Read [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md)
2. Review security section
3. Set up with small amounts first
4. Use [PR_QUICK_REFERENCE.md](PR_QUICK_REFERENCE.md) daily

### For Contributors
1. Read [PULL_REQUEST_TEMPLATE.md](.github/PULL_REQUEST_TEMPLATE.md)
2. Understand the validation process
3. Review example PRs in the guide

---

## ⚙️ Workflow Comparison

| Feature | Wrap/Unwrap | PR Distribution |
|---------|-------------|-----------------|
| **Trigger** | Manual | PR approval |
| **Purpose** | Personal use | Community distribution |
| **Automation** | On-demand | Approval-based |
| **Best For** | Individual wallets | Faucet/distribution |
| **Complexity** | Simple | Moderate |
| **Setup Time** | 5 minutes | 15 minutes |

---

## 🌐 Network Information

**Ethereum Classic Mordor Testnet**
- Chain ID: 63
- RPC: https://rpc.mordor.etccooperative.org
- Explorer: https://etc-mordor.blockscout.com
- WETC Contract: 0x1953cab0E5bFa6D4a9BaD6E05fD46C1CC6527a5a

---

## 🛟 Getting Help

### Common Questions

**Q: Which workflow should I use?**
A: Use wrap/unwrap for personal use. Use PR distribution for community faucets.

**Q: How do I get started quickly?**
A: Read [QUICKSTART.md](QUICKSTART.md) for wrap/unwrap or [PR_QUICK_REFERENCE.md](PR_QUICK_REFERENCE.md) for distribution.

**Q: Where do I add my private key?**
A: Settings → Secrets and variables → Actions → New repository secret

**Q: How much does it cost?**
A: Only gas fees on Mordor testnet (negligible).

**Q: Can I use this on mainnet?**
A: ⚠️ **NO!** This is for Mordor testnet only. Never use testnet setups on mainnet.

### Troubleshooting

See troubleshooting sections in:
- [README.md](README.md) - For wrap/unwrap issues
- [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md) - For distribution issues

---

## 🔒 Security Reminders

- ✅ Never commit private keys
- ✅ Use GitHub Secrets for sensitive data
- ✅ Test with small amounts first
- ✅ Monitor distribution wallet balance
- ✅ Review PRs before approving
- ✅ Keep limited funds in distribution wallet
- ⚠️ This is for testnet only - never use on mainnet

---

## 🎯 Next Steps

Choose your path:

**I want to wrap my own tokens:**
→ Go to [QUICKSTART.md](QUICKSTART.md)

**I want to create a faucet:**
→ Go to [PR_DISTRIBUTION_GUIDE.md](PR_DISTRIBUTION_GUIDE.md)

**I want to request tokens:**
→ Open a Pull Request using the template

**I need detailed setup commands:**
→ Go to [SETUP_COMMANDS.md](SETUP_COMMANDS.md)

**I need a quick reference:**
→ Go to [PR_QUICK_REFERENCE.md](PR_QUICK_REFERENCE.md)

---

## 📞 Resources

- [Ethereum Classic Website](https://ethereumclassic.org/)
- [Mordor Testnet GitHub](https://github.com/etclabscore/mordor)
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [Ethers.js Docs](https://docs.ethers.org/)
- [Blockscout Explorer](https://etc-mordor.blockscout.com/)

---

**Ready to get started?** Pick your use case above and follow the appropriate guide!
