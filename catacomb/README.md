# 🔐 Safe{Core} Catacomb - File-Based Token Distribution

**Secure, conflict-free token distribution on Ethereum Classic using Safe{Core} Catacomb.**

## 🎯 What is This?

A GitHub Actions workflow that distributes tokens through file-based Pull Requests, using Safe{Core} 2-of-3 multisig on Ethereum Classic Catacomb platform.

**Key Features:**
- 📁 **File-Based Requests** - No PR conflicts!
- 🔒 **2-of-3 Multisig** - Safe{Core} security
- 🤖 **Automated** - Proposes & executes automatically
- ✅ **Early Validation** - Fails fast, doesn't block other PRs
- 🏛️ **Official Platform** - ETC Cooperative's Catacomb

---

## 🆕 What's Different?

### Old Approach: PR Description
```
❌ All users edit PR text
❌ Merge conflicts between PRs
❌ One invalid PR blocks others
❌ Hard to track in Git history
```

### New Approach: Request Files ✨
```
✅ Each user creates unique file
✅ No conflicts (different files)
✅ Invalid requests don't block others
✅ Clean Git history per user
✅ Early validation & exit
```

---

## 📁 File Structure

```
your-repo/
├── .github/
│   ├── workflows/
│   │   └── catacomb-safe-distribution.yml
│   └── PULL_REQUEST_TEMPLATE.md
└── requests/
    ├── .gitkeep
    ├── alice.txt      → 0xAliceAddress
    ├── bob.txt        → 0xBobAddress
    └── charlie.txt    → 0xCharlieAddress
```

**Each file contains ONE wallet address.**

---

## 🚀 Quick Start

**30-minute setup:**

1. **Deploy Safe{Core}** on [multisig.etccooperative.org](https://multisig.etccooperative.org/)
2. **Configure GitHub** with SIGNER_1_KEY, SIGNER_2_KEY, SAFE_ADDRESS
3. **Create requests/ directory** in your repo
4. **Copy workflow** and PR template
5. **Test** with your own address
6. **Go live!**

📖 **Full Guide:** [CATACOMB_QUICKSTART.md](CATACOMB_QUICKSTART.md)

---

## 📊 How It Works

### User Flow

```
1. User forks repo
2. User creates: requests/username.txt
3. User adds wallet address to file
4. User opens PR
5. Bot validates file & address (< 10 sec)
6. Signer 1 proposes to Safe (< 30 sec)
7. Status: 1/2 signatures
8. Maintainer reviews & approves
9. Signer 2 executes (< 1 min)
10. Status: 2/2 signatures → EXECUTED
11. Tokens sent!
```

### Validation Flow

```
PR Opened
  ↓
Check: File in requests/ ?
  ├─ NO → Comment & exit (other PRs continue)
  └─ YES → Continue
  ↓
Check: Valid address?
  ├─ NO → Comment & exit (other PRs continue)
  └─ YES → Continue
  ↓
Propose to Safe
  ↓
Wait for approval
  ↓
Execute on approval
```

---

## 🔐 Security Model

### Safe{Core} Configuration

```
Platform: multisig.etccooperative.org (Catacomb)
Network: Ethereum Classic Mordor

Safe Owners:
├─ Signer 1 (GitHub Secret) → Proposes transactions
├─ Signer 2 (GitHub Secret) → Executes after approval
└─ Signer 3 (Offline)       → Emergency recovery

Threshold: 2 of 3 signatures required
```

### Why This is Secure

**Single-Key Wallet:**
```
1 key compromised → 🔴 Funds lost
```

**Safe{Core} 2-of-3:**
```
1 key compromised → ✅ Funds safe
2 keys compromised → ⚠️ Signer 3 can intervene
All transactions visible on-chain
```

---

## ⚙️ Configuration

### Required Secrets
```
SIGNER_1_KEY - First signer private key
SIGNER_2_KEY - Second signer private key
```

### Required Variables
```
SAFE_ADDRESS - Your Safe address from Catacomb
TOKEN_AMOUNT - Amount per request (e.g., "1.0")
```

### Optional Variables
```
WETC_ADDRESS - Token contract (default: WETC on Mordor)
```

---

## 📋 Files Included

| File | Purpose |
|------|---------|
| `catacomb-safe-distribution.yml` | Main workflow |
| `PULL_REQUEST_TEMPLATE.md` | User instructions |
| `requests/.gitkeep` | Directory marker |
| `requests/example.txt` | Example request file |
| `CATACOMB_SETUP_GUIDE.md` | Complete setup guide |
| `CATACOMB_QUICKSTART.md` | 30-minute quick start |
| `README.md` | This file |

---

## ✨ Key Benefits

### For Users
- ✅ Simple: Just create one file
- ✅ Clear: Obvious what to do
- ✅ Fast: Validated in seconds
- ✅ Reliable: No conflicts with other requests

### For Maintainers
- ✅ Automated: Minimal manual work
- ✅ Secure: 2-of-3 multisig
- ✅ Scalable: Handles many concurrent PRs
- ✅ Auditable: Git history per user

### Technical
- ✅ Early exit: Invalid requests don't block workflow
- ✅ No conflicts: Each user modifies different file
- ✅ Safe integration: Official ETC Cooperative platform
- ✅ Standards: Uses Safe{Core} (industry standard)

---

## 🎓 Terminology

**Safe{Core}**
- Formerly "Gnosis Safe"
- Industry-standard multisig wallet
- Battle-tested security

**Catacomb**
- Safe{Core} deployment on Ethereum Classic
- Official platform: [multisig.etccooperative.org](https://multisig.etccooperative.org/)
- Supports both mainnet and Mordor

**2-of-3 Multisig**
- 3 total owners
- 2 signatures required to execute
- 1 key can be lost, still works
- 1 key compromised, funds still safe

---

## 🛠️ Example Request File

**File:** `requests/alice.txt`

**Content:**
```
0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb
```

**Rules:**
- ✅ File in `requests/` folder
- ✅ Filename: `username.txt`
- ✅ Content: One wallet address
- ✅ Address format: `0x` + 40 hex chars
- ❌ No other content

---

## 📈 Comparison

| Feature | Single-Key | Old PR Method | File-Based Safe{Core} |
|---------|------------|---------------|----------------------|
| **Security** | 🔒 Low | 🔒🔒 Medium | 🔒🔒🔒 High |
| **Conflicts** | N/A | ❌ Yes | ✅ No |
| **Blocks PRs** | N/A | ❌ Yes | ✅ No |
| **Early Exit** | N/A | ❌ No | ✅ Yes |
| **Recovery** | ❌ None | ❌ None | ✅ Offline key |
| **Platform** | N/A | N/A | ✅ Official (Catacomb) |

---

## 🔄 Workflow Triggers

```yaml
on:
  pull_request:
    types: [opened, reopened, synchronize]
    paths:
      - 'requests/**'  # Only triggers on requests/ changes
  pull_request_review:
    types: [submitted]
```

**Benefits:**
- Only runs when requests/ files change
- Doesn't run for other PR changes
- Efficient use of GitHub Actions

---

## 🌐 Network Information

**Ethereum Classic Mordor Testnet**
- Chain ID: 63
- RPC: https://rpc.mordor.etccooperative.org
- Explorer: https://etc-mordor.blockscout.com
- WETC: 0x1953cab0E5bFa6D4a9BaD6E05fD46C1CC6527a5a

**Safe{Core} Catacomb**
- Platform: https://multisig.etccooperative.org
- Operator: ETC Cooperative
- Technology: Safe{Core} (official)

---

## 📚 Documentation

**Quick Start:**
- [CATACOMB_QUICKSTART.md](CATACOMB_QUICKSTART.md) - 30-minute setup

**Complete Guide:**
- [CATACOMB_SETUP_GUIDE.md](CATACOMB_SETUP_GUIDE.md) - Detailed instructions

**Platform:**
- [multisig.etccooperative.org](https://multisig.etccooperative.org/) - Deploy & manage Safe

---

## 🆘 Troubleshooting

**"No request file found"**
→ Create file in `requests/username.txt`

**"Invalid wallet address"**
→ Format: `0x` + 40 hex characters

**"Signer not an owner"**
→ Check Safe owners on Catacomb platform

**"Insufficient balance"**
→ Send more WETC to Safe address

📖 See [CATACOMB_SETUP_GUIDE.md](CATACOMB_SETUP_GUIDE.md) for more

---

## ⚠️ Important Notes

- 🔴 **Testnet only** - For Mordor testnet use
- 🔴 **Official platform** - Use multisig.etccooperative.org
- 🔴 **Terminology** - Say "Safe{Core}" not "Gnosis Safe"
- 🔴 **Catacomb** - ETC deployment name
- 🔴 **File-based** - No PR description editing
- 🔴 **Keep Signer 3 offline** - Hardware wallet recommended

---

## 🎯 Next Steps

**New users:**
1. Read [CATACOMB_QUICKSTART.md](CATACOMB_QUICKSTART.md)
2. Deploy Safe on Catacomb
3. Test with small amount
4. Go live!

**Migrating:**
1. Update workflow to file-based version
2. Update PR template
3. Create requests/ directory
4. Announce changes to users

---

## 🤝 Resources

- [Safe{Core} Docs](https://docs.safe.global/)
- [ETC Cooperative](https://etccooperative.org/)
- [Mordor Testnet](https://github.com/etclabscore/mordor)
- [Blockscout Explorer](https://etc-mordor.blockscout.com/)

---

## 📄 License

MIT License - Educational use on testnet only.

---

**Ready to start?** 

→ [CATACOMB_QUICKSTART.md](CATACOMB_QUICKSTART.md) for 30-minute setup!

→ [CATACOMB_SETUP_GUIDE.md](CATACOMB_SETUP_GUIDE.md) for complete guide!

---

**Built for the Ethereum Classic community with ❤️**