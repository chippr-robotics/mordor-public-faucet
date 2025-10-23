# 🔐 Gnosis Safe Multisig Token Distribution

**Secure, transparent, automated token distribution using a 2-of-3 Gnosis Safe multisig as treasury.**

## 🎯 What is This?

A GitHub Actions workflow that distributes tokens through Pull Request approvals, using a Gnosis Safe multisig for enhanced security.

**Key Features:**
- 🔒 **2-of-3 Multisig Security** - Requires 2 signatures to execute
- 🤖 **Automated Workflow** - Proposes on PR open, executes on approval
- 👥 **Human Review** - PR approval required before execution
- 📊 **Transparent** - All transactions visible on-chain
- 🛡️ **Recovery Options** - Third key offline for emergencies

## 🚀 Quick Start

**Choose your path:**

- 🏃 **Fast Track** → [SAFE_QUICKSTART.md](SAFE_QUICKSTART.md) (30 minutes)
- 📖 **Complete Guide** → [SAFE_SETUP_GUIDE.md](SAFE_SETUP_GUIDE.md) (detailed)
- ❓ **Questions** → [SAFE_FAQ.md](SAFE_FAQ.md) (common issues)

## 📊 How It Works

### Two-Step Process

**Step 1: PR Submitted**
```
User opens PR → Signer 1 proposes to Safe → Status: 1/2 signatures
```

**Step 2: PR Approved**
```
Maintainer approves → Signer 2 executes → Status: 2/2 signatures → Tokens sent!
```

### Security Model

```
Gnosis Safe (2-of-3 multisig)
├─ Signer 1 (GitHub) → Proposes transactions
├─ Signer 2 (GitHub) → Executes after approval
└─ Signer 3 (Offline) → Emergency recovery
```

## 📁 Documentation

### Getting Started
| Document | Description | Time |
|----------|-------------|------|
| [SAFE_QUICKSTART.md](SAFE_QUICKSTART.md) | Fast setup guide | 30 min |
| [SAFE_SETUP_GUIDE.md](SAFE_SETUP_GUIDE.md) | Complete instructions | 1 hour |
| [SAFE_DEPLOYMENT_SCRIPT.md](SAFE_DEPLOYMENT_SCRIPT.md) | Deploy your Safe | 15 min |

### Reference
| Document | Description |
|----------|-------------|
| [SAFE_FAQ.md](SAFE_FAQ.md) | Common questions & answers |
| [SAFE_WORKFLOW_DIAGRAM.md](SAFE_WORKFLOW_DIAGRAM.md) | Visual flowcharts |

### Files
| File | Purpose |
|------|---------|
| `.github/workflows/safe-token-distribution.yml` | Main workflow |
| `.github/PULL_REQUEST_TEMPLATE.md` | User request template |

## 🔧 Setup Requirements

### Required Secrets (GitHub)
```
SIGNER_1_KEY - First signer private key (proposes)
SIGNER_2_KEY - Second signer private key (executes)
```

### Required Variables (GitHub)
```
SAFE_ADDRESS - Your deployed Safe address
TOKEN_AMOUNT - Amount per distribution (e.g., "1.0")
```

### Optional Variables
```
WETC_ADDRESS - Token contract (default: WETC on Mordor)
```

## ✨ Features

### Security
- ✅ 2 signatures required for execution
- ✅ No single point of failure
- ✅ Offline recovery key
- ✅ All transactions on-chain
- ✅ Automated yet secure

### Workflow
- ✅ Automatic address validation
- ✅ Duplicate detection
- ✅ PR comments with status
- ✅ Smart labels for tracking
- ✅ Error handling & retries

### User Experience
- ✅ Simple PR submission
- ✅ Instant validation
- ✅ Clear status updates
- ✅ Automatic execution
- ✅ Transaction confirmation

## 📈 Comparison

### vs Single-Key Wallet

| Feature | Single Key | Safe Multisig |
|---------|------------|---------------|
| **Security** | Single point of failure | 2 keys required |
| **If compromised** | Funds lost | Still secure |
| **Recovery** | None | Offline key |
| **Complexity** | Low | Medium |
| **Setup time** | 5 min | 30 min |

**Recommendation:** Use Safe for any treasury > 100 tokens

### vs 2-of-2 Safe

| Feature | 2-of-2 | 2-of-3 |
|---------|--------|--------|
| **If key lost** | Funds locked | Still works |
| **Recovery** | Impossible | Third key |
| **Redundancy** | None | One backup |

**Recommendation:** Always use 2-of-3 for better redundancy

## 🛠️ Typical Setup Flow

```bash
# 1. Deploy Safe (10 min)
export DEPLOYER_KEY="0x..."
export OWNER_1_ADDRESS="0x..."
export OWNER_2_ADDRESS="0x..."
export OWNER_3_ADDRESS="0x..."
node deploy-safe.js

# 2. Fund Safe (2 min)
# Send WETC to Safe address

# 3. Configure GitHub (5 min)
# Add SIGNER_1_KEY, SIGNER_2_KEY secrets
# Add SAFE_ADDRESS, TOKEN_AMOUNT variables

# 4. Deploy workflow (5 min)
cp .github/workflows/safe-token-distribution.yml YOUR_REPO/.github/workflows/

# 5. Test (3 min)
# Create test PR, approve, verify tokens sent

# 6. Go live! (0 min)
# Announce to community
```

## 🎓 Learning Resources

### For Beginners
1. Read [SAFE_QUICKSTART.md](SAFE_QUICKSTART.md)
2. Follow step-by-step instructions
3. Test with small amounts
4. Check [SAFE_FAQ.md](SAFE_FAQ.md) for issues

### For Maintainers
1. Understand [SAFE_WORKFLOW_DIAGRAM.md](SAFE_WORKFLOW_DIAGRAM.md)
2. Read [SAFE_SETUP_GUIDE.md](SAFE_SETUP_GUIDE.md) security section
3. Set up monitoring
4. Plan incident response

### For Developers
1. Review workflow YAML
2. Understand Safe SDK
3. Customize for your needs
4. Add additional features

## 🔒 Security Highlights

### Built-In Protections
- ✅ **Multisig Required** - 2 keys to execute
- ✅ **Human Review** - PR approval gate
- ✅ **Address Validation** - Format checking
- ✅ **Duplicate Prevention** - One distribution per PR
- ✅ **On-Chain Audit Trail** - All visible on Blockscout
- ✅ **Emergency Recovery** - Offline Signer 3

### Best Practices
- 🔑 Keep Signer 3 offline (hardware wallet)
- 👀 Review every PR carefully
- 📊 Monitor Safe balance
- 🚨 Set up alerts for unusual activity
- 📝 Document all keys securely
- 🔄 Regular security audits

### Attack Scenarios
- **Signer 1 compromised** → ✅ Safe (can't execute alone)
- **Signer 2 compromised** → ✅ Safe (can't propose)
- **Both compromised** → ⚠️ Signer 3 can intervene
- **GitHub hacked** → ⚠️ Signer 3 removes compromised keys
- **Spam PRs** → ✅ Human review required

## 📊 Example Usage

### User Flow
```
1. User opens PR with wallet address
   → Bot validates (5 sec)
   
2. Signer 1 proposes to Safe
   → Comment: "Proposed (1/2)"
   
3. Maintainer reviews PR
   → Checks legitimacy
   
4. Maintainer approves PR
   → Signer 2 executes (30 sec)
   
5. Tokens sent!
   → Comment: "Executed successfully!"
```

### Maintainer Flow
```
1. Review new PRs
   → Check if legitimate
   
2. Approve good requests
   → Automatic execution
   
3. Monitor execution
   → Verify on Blockscout
   
4. Close completed PRs
   → Archive for records
```

## 🆘 Getting Help

**Quick answers:** [SAFE_FAQ.md](SAFE_FAQ.md)

**Setup issues:** [SAFE_SETUP_GUIDE.md](SAFE_SETUP_GUIDE.md)

**Deployment:** [SAFE_DEPLOYMENT_SCRIPT.md](SAFE_DEPLOYMENT_SCRIPT.md)

**Visual guide:** [SAFE_WORKFLOW_DIAGRAM.md](SAFE_WORKFLOW_DIAGRAM.md)

**Still stuck?** Open an issue with:
- What you're trying to do
- What happened (error messages)
- Your configuration (without secrets!)

## ⚠️ Important Warnings

- 🔴 **TESTNET ONLY** - This is for Mordor testnet
- 🔴 **Never use on mainnet** without professional security audit
- 🔴 **Keep private keys secure** - Never commit to Git
- 🔴 **Test thoroughly** before going live
- 🔴 **Monitor actively** after deployment
- 🔴 **Have incident response plan** ready

## 🌐 Network Information

**Ethereum Classic Mordor Testnet**
- Chain ID: 63
- RPC: https://rpc.mordor.etccooperative.org
- Explorer: https://etc-mordor.blockscout.com
- WETC: 0x1953cab0E5bFa6D4a9BaD6E05fD46C1CC6527a5a

## 📚 Additional Resources

### Gnosis Safe
- [Safe Documentation](https://docs.safe.global/)
- [Protocol Kit SDK](https://docs.safe.global/safe-core-aa-sdk/protocol-kit)
- [Safe Contracts](https://github.com/safe-global/safe-contracts)

### Ethereum Classic
- [ETC Website](https://ethereumclassic.org/)
- [Mordor Testnet](https://github.com/etclabscore/mordor)
- [Blockscout Explorer](https://etc-mordor.blockscout.com/)

### GitHub Actions
- [Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Additional security features
- Better error handling
- More automation
- Documentation improvements
- Bug fixes

## 📄 License

MIT License - Educational use on testnet only.

---

## 🎯 Next Steps

**New to this?** Start here:
1. Read [SAFE_QUICKSTART.md](SAFE_QUICKSTART.md)
2. Deploy your Safe
3. Test with small amounts
4. Go live!

**Ready to deploy?** Follow:
1. [SAFE_DEPLOYMENT_SCRIPT.md](SAFE_DEPLOYMENT_SCRIPT.md) - Deploy Safe
2. [SAFE_SETUP_GUIDE.md](SAFE_SETUP_GUIDE.md) - Complete setup
3. [SAFE_FAQ.md](SAFE_FAQ.md) - Troubleshooting

**Need help?** Check:
1. [SAFE_FAQ.md](SAFE_FAQ.md) - Common questions
2. Workflow logs - Action tab
3. Open an issue - With details

---

**Ready to build a secure token distribution system?** Start with [SAFE_QUICKSTART.md](SAFE_QUICKSTART.md)! 🚀
