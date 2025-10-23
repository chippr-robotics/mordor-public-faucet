# Gnosis Safe Token Distribution - FAQ

Common questions and answers about the Gnosis Safe multisig token distribution system.

## General Questions

### Q: What is a Gnosis Safe?
**A:** A smart contract wallet that requires multiple signatures (multisig) to execute transactions. It's one of the most secure and battle-tested multisig solutions in Web3.

### Q: Why use a Safe instead of a regular wallet?
**A:** Security benefits:
- No single point of failure (requires 2 keys)
- If one key is compromised, funds are still safe
- Transparent on-chain operations
- Industry standard for treasury management

### Q: What is 2-of-3 multisig?
**A:** 
- 3 total owner addresses
- Any 2 owners can execute a transaction
- Provides redundancy (one key can be lost)
- Prevents single-key compromise

---

## Setup Questions

### Q: Do I need to deploy my own Safe?
**A:** Yes, you need to deploy a Safe on Mordor testnet. See `SAFE_DEPLOYMENT_SCRIPT.md` for instructions.

### Q: Can I use an existing Safe?
**A:** Yes! If you already have a Safe deployed, just configure the SAFE_ADDRESS variable to point to it.

### Q: How much does it cost to deploy a Safe?
**A:** On Mordor testnet, very little (just gas fees, typically < 0.1 ETC).

### Q: Can I use a different threshold (e.g., 2-of-2 or 3-of-5)?
**A:** Yes, but you'll need to modify the workflow logic. The current setup is optimized for 2-of-3.

### Q: What if Safe contracts aren't deployed on Mordor?
**A:** You can:
1. Deploy Safe contracts yourself from their GitHub
2. Use a different multisig solution
3. Wait for official Mordor support

---

## Configuration Questions

### Q: Where do I store the three private keys?
**A:**
- Signer 1: GitHub Secrets (`SIGNER_1_KEY`)
- Signer 2: GitHub Secrets (`SIGNER_2_KEY`)
- Signer 3: Hardware wallet or secure offline storage (NOT in GitHub)

### Q: Can I use the same key for both signers?
**A:** Technically yes, but this defeats the purpose of multisig. Always use different keys for each signer.

### Q: What if I lose Signer 3's key?
**A:** That's okay! Your Safe only needs 2 signatures. Signer 1 + Signer 2 can still operate. But you should add a new Signer 3 for redundancy.

### Q: How do I rotate keys?
**A:** Use the Safe interface or SDK to:
1. Add new owner
2. Remove old owner
3. Update GitHub Secrets if needed

### Q: Can I have more than 3 signers?
**A:** Yes! You can create a 2-of-N safe. Just modify the deployment and threshold.

---

## Workflow Questions

### Q: What happens when a PR is opened?
**A:**
1. Bot validates wallet address
2. Signer 1 proposes transaction to Safe
3. Transaction signed but NOT executed (waiting for signature #2)
4. PR labeled as `safe-proposed`

### Q: What happens when a PR is approved?
**A:**
1. Signer 2 detects approval
2. Signer 2 signs the same transaction
3. Transaction now has 2/2 signatures
4. Safe executes transaction automatically
5. Tokens sent to recipient

### Q: Can I approve multiple PRs at once?
**A:** Each PR creates a separate Safe transaction, so yes. They process independently.

### Q: What if I accidentally approve a malicious PR?
**A:** The transaction is already signed by Signer 1, so approval will execute it. Always review carefully!

### Q: Can I cancel a proposed transaction?
**A:** Yes, before approval:
- Close the PR
- Or use Signer 3 to reject the transaction via Safe interface

### Q: What if both GitHub signers are compromised?
**A:** Use Signer 3 (offline) to:
- Remove compromised signers
- Add new signers
- Reject any pending malicious transactions

---

## Security Questions

### Q: Is this setup secure?
**A:** Yes, for testnet use. Benefits:
- 2 signatures required (no single point of failure)
- One offline key as backup
- All transactions visible on-chain

### Q: What if GitHub is hacked?
**A:** 
- Attacker could get Signer 1 and Signer 2 keys
- They could execute transactions together
- But: All transactions are public, visible on Blockscout
- Signer 3 (offline) can intervene and remove them

### Q: Should I use this on mainnet?
**A:** **NO!** This is for testnet education only. For mainnet:
- Use hardware wallets for all signers
- Consider higher threshold (3-of-5 or more)
- Get professional security audit
- Implement rate limiting
- Add spending limits
- Use dedicated secure infrastructure

### Q: How do I monitor for suspicious activity?
**A:**
- Watch Safe address on Blockscout
- Set up alerts for transactions
- Review all PRs carefully before approval
- Check GitHub Action logs regularly

### Q: What if I see a suspicious proposed transaction?
**A:**
1. Don't approve the PR
2. Close the PR
3. Investigate the request
4. Use Signer 3 to reject if needed

---

## Troubleshooting Questions

### Q: "Signer 1 is not an owner of this Safe"
**A:** 
1. Get address from SIGNER_1_KEY:
   ```bash
   node -e "console.log(new (require('ethers').Wallet)('KEY').address)"
   ```
2. Compare with Safe owners on Blockscout
3. Either update the Safe or use correct key

### Q: "Insufficient Safe balance"
**A:** 
1. Check Safe balance on Blockscout
2. Send more WETC to Safe address
3. Wait for confirmation
4. Retry

### Q: Transaction fails to execute
**A:** Common causes:
- Insufficient Safe balance
- Wrong token address
- Network issues
- Gas estimation problems

Check workflow logs for specific error.

### Q: "Transaction already executed"
**A:** This is correct behavior (prevents double-send). The PR already completed successfully.

### Q: Workflow doesn't trigger
**A:** Check:
- Workflow file is in `.github/workflows/`
- Actions are enabled in repository
- Workflow permissions are correct
- YAML syntax is valid

### Q: Can't connect to Safe
**A:** Verify:
- SAFE_ADDRESS is correct
- Safe is deployed on Mordor
- RPC endpoint is working
- Network connectivity is good

---

## Advanced Questions

### Q: Can I batch multiple distributions?
**A:** Yes! You can modify the workflow to create batch transactions. See Safe SDK docs.

### Q: Can I add rate limiting?
**A:** Yes, add checks in the workflow:
```yaml
- name: Check Rate Limit
  # Check if user requested recently
  # Fail if within rate limit window
```

### Q: Can I require multiple approvers on GitHub?
**A:** Yes! Use branch protection rules:
- Settings → Branches → Add rule
- Require 2+ approvals
- This adds a human layer before Signer 2 executes

### Q: Can I integrate with Discord/Slack?
**A:** Yes! Add notification steps to the workflow:
```yaml
- name: Notify
  run: curl -X POST $WEBHOOK_URL -d '{"text":"TX proposed"}'
```

### Q: Can I use this for other tokens or ETC?
**A:** Yes! 
- For other tokens: Change WETC_ADDRESS variable
- For native ETC: Modify transaction to send ETH value
- Any ERC-20 token works

### Q: How do I upgrade the Safe version?
**A:** Safe upgrades are complex. Options:
1. Deploy new Safe and migrate funds
2. Use Safe's upgrade mechanism (if available)
3. Consult Safe documentation

### Q: Can I add time locks?
**A:** Not natively in this setup. You'd need to:
- Deploy a timelock contract
- Make Safe execute through timelock
- More complex setup

### Q: What if Mordor network goes down?
**A:** 
- Workflow will fail (can't connect)
- Safe and funds are still secure
- Retry when network is back
- Use alternative RPC if available

---

## Comparison Questions

### Q: Safe vs regular wallet for distribution?
**A:**

**Regular Wallet:**
- ✅ Simpler setup
- ✅ Faster execution
- ❌ Single point of failure
- ❌ If compromised, funds gone

**Safe Multisig:**
- ✅ More secure (2 keys needed)
- ✅ Recovery options
- ✅ Industry standard
- ❌ More complex setup
- ❌ Slightly higher gas costs

### Q: 2-of-3 vs 2-of-2 Safe?
**A:**

**2-of-2:**
- ❌ If one key lost, funds locked forever
- ✅ Simpler (only 2 keys to manage)

**2-of-3:**
- ✅ Can lose one key and still operate
- ✅ Better redundancy
- ❌ One more key to secure

Recommendation: Use 2-of-3 for better redundancy.

---

## Best Practices

### Q: What are the best practices for Safe management?
**A:**
1. ✅ Keep Signer 3 offline (hardware wallet)
2. ✅ Regularly verify Safe owners
3. ✅ Monitor all transactions
4. ✅ Test with small amounts first
5. ✅ Document all keys securely
6. ✅ Have recovery plan
7. ✅ Regular security audits
8. ✅ Keep Safe funded but not over-funded

### Q: How often should I review configurations?
**A:** 
- Monthly: Review owners, threshold
- Weekly: Check balance, pending transactions
- Daily: Monitor GitHub Actions logs
- Immediately: After any suspicious activity

### Q: Should I announce the Safe address publicly?
**A:** 
- ✅ Yes - transparency is good
- ✅ Users can verify funds exist
- ✅ Anyone can audit transactions
- ❌ Don't share owner addresses publicly

---

## Getting Help

### Q: Where can I get more help?
**A:**
- Read `SAFE_SETUP_GUIDE.md` for detailed instructions
- Check workflow logs in Actions tab
- Review Safe docs: https://docs.safe.global
- Ask in Ethereum Classic communities
- Check GitHub Issues on Safe repos

### Q: How do I report a bug?
**A:**
1. Check workflow logs
2. Verify configuration
3. Test with small amounts
4. Document the issue
5. Create GitHub Issue with details

### Q: Can I contribute improvements?
**A:** Yes! Pull requests welcome for:
- Better error handling
- Additional security features
- Documentation improvements
- Bug fixes

---

## Quick Reference

**Deploy Safe:**
```bash
node deploy-safe.js
```

**Get Address from Key:**
```bash
node -e "console.log(new (require('ethers').Wallet)('KEY').address)"
```

**Check Safe Owners:**
```bash
# Via Blockscout
https://etc-mordor.blockscout.com/address/YOUR_SAFE
```

**Required Secrets:**
- `SIGNER_1_KEY`
- `SIGNER_2_KEY`

**Required Variables:**
- `SAFE_ADDRESS`
- `TOKEN_AMOUNT`

**Optional Variables:**
- `WETC_ADDRESS`

---

**Still have questions?** Open an issue or check the full documentation!
