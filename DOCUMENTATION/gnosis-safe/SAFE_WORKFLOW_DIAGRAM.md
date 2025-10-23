# 🎨 Gnosis Safe Multisig Workflow - Visual Guide

## Overview: 2-Step Process

```
┌─────────────────────────────────────────────────────────────────┐
│  STEP 1: PROPOSE (When PR Opens)                                │
│  ├─ Signer 1 proposes transaction                               │
│  ├─ Transaction signed but NOT executed                         │
│  └─ Status: 1/2 signatures                                      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  STEP 2: EXECUTE (When PR Approved)                             │
│  ├─ Signer 2 adds second signature                              │
│  ├─ Safe automatically executes                                 │
│  └─ Status: 2/2 signatures → EXECUTED                           │
└─────────────────────────────────────────────────────────────────┘
```

---

## Complete User Journey

```
┌──────────────────────────────────────────────────────────────────┐
│                     USER OPENS PULL REQUEST                       │
│  Title: Token Request: 0xABC123...                               │
│  Body:  Address: 0xABC123...                                     │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  VALIDATION (< 5 seconds)                                         │
│  ├─ Extract wallet address with regex                            │
│  ├─ Validate format (0x + 40 hex)                                │
│  └─ Post validation comment                                      │
└──────────────────────────────────────────────────────────────────┘
                              │
                    ┌─────────┴─────────┐
                    │                   │
            ✅ Valid           ❌ Invalid
                    │                   │
                    ▼                   ▼
        ┌─────────────────┐   ┌──────────────────┐
        │ Continue flow   │   │ Request fix      │
        └─────────────────┘   │ Label: invalid   │
                              └──────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────────────────────────┐
│  STEP 1: PROPOSE TO SAFE (< 30 seconds)                          │
│                                                                   │
│  ┌────────────────────────────────────────────────────────┐     │
│  │  1. Connect to Safe with Signer 1                      │     │
│  │  2. Verify Signer 1 is owner                           │     │
│  │  3. Check Safe balance                                 │     │
│  │  4. Create Safe transaction (token transfer)           │     │
│  │  5. Sign with Signer 1                                 │     │
│  │  6. Transaction proposed (NOT executed)                │     │
│  └────────────────────────────────────────────────────────┘     │
│                                                                   │
│  Safe Status: Pending (1/2 signatures)                           │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  POST PROPOSAL COMMENT                                            │
│                                                                   │
│  ✅ Transaction Proposed to Gnosis Safe                          │
│                                                                   │
│  Status: Awaiting Approval (1/2 signatures)                      │
│  - ✍️ Signature 1: ✅ Applied                                    │
│  - ✍️ Signature 2: ⏳ Awaiting review                            │
│                                                                   │
│  Labels: [safe-proposed] [pending-review]                        │
└──────────────────────────────────────────────────────────────────┘
                              │
                              │
                    ┌─────────┴──────────┐
                    │  WAITING FOR       │
                    │  HUMAN REVIEW      │
                    │  (minutes to days) │
                    └─────────┬──────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│              MAINTAINER REVIEWS PULL REQUEST                      │
│                                                                   │
│  Checks:                                                          │
│  ├─ Is request legitimate?                                       │
│  ├─ Is address correct?                                          │
│  ├─ Not a duplicate?                                             │
│  └─ User intent reasonable?                                      │
└──────────────────────────────────────────────────────────────────┘
                              │
                    ┌─────────┴─────────┐
                    │                   │
            ✅ APPROVE          ❌ REJECT
                    │                   │
                    │                   └─→ Close PR
                    ▼
┌──────────────────────────────────────────────────────────────────┐
│  STEP 2: EXECUTE TRANSACTION (< 1 minute)                        │
│                                                                   │
│  ┌────────────────────────────────────────────────────────┐     │
│  │  1. Detect PR approval                                 │     │
│  │  2. Extract wallet address                             │     │
│  │  3. Check not already executed                         │     │
│  │  4. Connect to Safe with Signer 2                      │     │
│  │  5. Verify Signer 2 is owner                           │     │
│  │  6. Recreate same transaction                          │     │
│  │  7. Sign with Signer 2 (second signature)              │     │
│  │  8. Execute transaction (2/2 signatures!)              │     │
│  │  9. Wait for blockchain confirmation                   │     │
│  └────────────────────────────────────────────────────────┘     │
│                                                                   │
│  Safe Status: Executed ✅                                        │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│  POST SUCCESS COMMENT                                             │
│                                                                   │
│  ✅ Transaction Executed Successfully!                           │
│                                                                   │
│  Status: Complete (2/2 signatures)                               │
│  - ✍️ Signature 1: ✅ Applied                                    │
│  - ✍️ Signature 2: ✅ Applied                                    │
│  - 🚀 Transaction: ✅ Executed                                   │
│                                                                   │
│  TX Hash: 0x...                                                  │
│  Block: 12345                                                    │
│                                                                   │
│  Labels: [safe-executed] [tokens-sent]                           │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│                    ✨ COMPLETE! ✨                                │
│  User has received tokens                                         │
│  PR can be closed                                                │
└──────────────────────────────────────────────────────────────────┘
```

---

## Security Model: 2-of-3 Multisig

```
┌─────────────────────────────────────────────────────────────────┐
│                        GNOSIS SAFE                               │
│                      (On-Chain Contract)                         │
│                                                                  │
│  Configuration:                                                  │
│  ├─ 3 Owners                                                    │
│  ├─ 2 Signatures Required (Threshold)                           │
│  └─ Token Balance: WETC                                         │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Owner 1: Signer 1 Address                               │  │
│  │  - Private Key in GitHub Secrets                         │  │
│  │  - Role: Proposes transactions                           │  │
│  │  - Usage: Every PR submission                            │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Owner 2: Signer 2 Address                               │  │
│  │  - Private Key in GitHub Secrets                         │  │
│  │  - Role: Executes transactions                           │  │
│  │  - Usage: Every PR approval                              │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Owner 3: Signer 3 Address                               │  │
│  │  - Private Key OFFLINE (hardware wallet)                 │  │
│  │  - Role: Emergency recovery, manual override             │  │
│  │  - Usage: Rare (emergencies only)                        │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
│  To Execute Any Transaction:                                    │
│  ├─ Option A: Signer 1 + Signer 2 (automated)                  │
│  ├─ Option B: Signer 1 + Signer 3 (manual)                     │
│  └─ Option C: Signer 2 + Signer 3 (manual)                     │
└─────────────────────────────────────────────────────────────────┘
```

---

## Attack Resistance

### Scenario 1: Signer 1 Compromised

```
❌ Attacker has Signer 1
     │
     ├─ Can propose transactions
     │
     └─ ❌ CANNOT execute (needs Signer 2 or 3)
           │
           └─ ✅ SAFE: Funds secure
```

### Scenario 2: Signer 2 Compromised

```
❌ Attacker has Signer 2
     │
     ├─ Cannot propose (only Signer 1 proposes)
     │
     └─ ❌ CANNOT execute alone (needs proposal first)
           │
           └─ ✅ SAFE: Funds secure
```

### Scenario 3: Both GitHub Signers Compromised

```
❌ Attacker has Signer 1 AND Signer 2
     │
     ├─ ⚠️ Can propose and execute transactions
     │
     ├─ BUT: All transactions visible on Blockscout
     │
     └─ ✅ Signer 3 (offline) can intervene:
           │
           ├─ Remove Signer 1 and 2
           ├─ Add new signers
           └─ Reject pending transactions
```

### Scenario 4: Spam Attack

```
💀 Attacker floods with spam PRs
     │
     ├─ Signer 1 proposes all of them
     │
     ├─ BUT: None execute (need approval)
     │
     └─ ✅ Human review filters spam
           │
           └─ Only legitimate PRs approved
```

---

## State Diagram

```
         [Safe Deployed]
              │
              ▼
    ┌──────────────────┐
    │  Awaiting Request│
    └────────┬─────────┘
             │
             │ (PR Opened)
             ▼
    ┌──────────────────┐
    │   Validating     │
    └────────┬─────────┘
             │
       ┌─────┴─────┐
       │           │
       ▼           ▼
  [Valid]     [Invalid]
       │           │
       │           └──→ [Request Fix] ──┐
       │                                 │
       ▼                                 │
 [Proposing]                            │
       │                                 │
       ▼                                 │
 [Proposed] ◄────────────────────────────┘
 (1/2 sigs)
       │
       │ (Awaiting Review)
       │
       ├──→ [Rejected] → [Closed]
       │
       │ (Approved)
       ▼
 [Executing]
 (2/2 sigs)
       │
       ├──→ [Failed] → [Manual Review]
       │
       ▼
 [Executed] ✅
       │
       ▼
   [Closed]
```

---

## Timeline Example

```
T+0:00:00  │ User creates PR
           │
T+0:00:05  │ ✅ Bot validates address
           │
T+0:00:10  │ ⚙️  Signer 1 proposes to Safe
           │
T+0:00:25  │ 💬 Comment: "Proposed (1/2 signatures)"
           │
           │ [WAITING FOR HUMAN REVIEW]
           │
T+2:15:30  │ 👤 Maintainer reviews PR
           │
T+2:15:45  │ ✅ Maintainer approves
           │
T+2:15:50  │ ⚙️  Signer 2 executes transaction
           │
T+2:16:20  │ ⛏️  Transaction mined (block confirmed)
           │
T+2:16:25  │ 💬 Comment: "Executed successfully!"
           │
T+2:16:26  │ ✨ Complete!

Total Active Time: ~1 minute
Total Wait Time: ~2 hours 15 minutes (for human review)
```

---

## Component Interaction

```
┌──────────────────────────────────────────────────────────┐
│                      GITHUB                              │
│                                                          │
│  ┌───────────────┐        ┌──────────────────┐         │
│  │ Pull Request  │───────▶│ GitHub Actions   │         │
│  │ (User Input)  │        │ (Automation)     │         │
│  └───────────────┘        └──────────┬───────┘         │
│         │                             │                 │
│         │      ┌──────────────────┐  │                 │
│         └─────▶│  PR Comments     │◀─┘                 │
│                │  (Bot Updates)   │                    │
│                └──────────────────┘                    │
└──────────────────────────────────────────────────────────┘
                         │
                         │ Web3 Calls
                         ▼
┌──────────────────────────────────────────────────────────┐
│            ETHEREUM CLASSIC MORDOR                        │
│                                                          │
│  ┌─────────────────────────────────────────────────┐   │
│  │          GNOSIS SAFE CONTRACT                   │   │
│  │                                                 │   │
│  │  ┌──────────────────────────────────────┐     │   │
│  │  │  Step 1: Signer 1 Proposes          │     │   │
│  │  │  - createTransaction()              │     │   │
│  │  │  - signTransaction()                │     │   │
│  │  │  - Status: 1/2 signatures           │     │   │
│  │  └──────────────────────────────────────┘     │   │
│  │                                                 │   │
│  │  ┌──────────────────────────────────────┐     │   │
│  │  │  Step 2: Signer 2 Executes          │     │   │
│  │  │  - signTransaction()                │     │   │
│  │  │  - executeTransaction()             │     │   │
│  │  │  - Status: 2/2 signatures ✅        │     │   │
│  │  └──────────────────────────────────────┘     │   │
│  │                                                 │   │
│  │  ⬇️ Calls ⬇️                                  │   │
│  └─────────────────────────────────────────────────┘   │
│                                                          │
│  ┌─────────────────────────────────────────────────┐   │
│  │          WETC TOKEN CONTRACT                    │   │
│  │  - transfer(recipient, amount)                  │   │
│  │  - Emit Transfer event                          │   │
│  └─────────────────────────────────────────────────┘   │
│                    │                                    │
│                    ▼                                    │
│  ┌─────────────────────────────────────────────────┐   │
│  │         USER WALLET                             │   │
│  │  - Receives tokens                              │   │
│  └─────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────┘
```

---

## Signature Collection Flow

```
Transaction Created
       │
       ▼
┌──────────────────┐
│  Signature 1/2   │
│  (Signer 1)      │
│                  │
│  Status: Pending │
│  Can Execute: NO │
└────────┬─────────┘
         │
         │ (After PR approval)
         ▼
┌──────────────────┐
│  Signature 2/2   │
│  (Signer 2)      │
│                  │
│  Status: Ready   │
│  Can Execute: YES│
└────────┬─────────┘
         │
         │ (Automatic)
         ▼
┌──────────────────┐
│    Execution     │
│  Safe performs   │
│  token transfer  │
│                  │
│  Status: Done ✅ │
└──────────────────┘
```

---

**Visual Legend:**
- `│ ▼ ►` - Flow direction
- `[State]` - System state
- `✅` - Success
- `❌` - Failure/Block
- `⚙️` - Automated action
- `👤` - Human action
- `💬` - Comment/notification
- `⏳` - Waiting
- `🔒` - Security checkpoint
- `⛏️` - Blockchain operation
