---
name: Token Request
about: Request testnet tokens by creating a request file
title: 'Token Request: [Your GitHub Username]'
labels: token-request
assignees: ''
---

## 📝 Token Request Instructions

To request tokens, please follow these steps:

### 1. Create Your Request File

Create a new file in the `requests/` folder with your GitHub username:

```
requests/YOUR_GITHUB_USERNAME.txt
```

**Example:** If your GitHub username is `alice`, create: `requests/alice.txt`

### 2. Add Your Wallet Address

In the file, add **only** your Ethereum Classic wallet address:

```
0xYourWalletAddressHere
```

**Example content of `requests/alice.txt`:**
```
0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb
```

### 3. Important Rules

- ✅ File must be in `requests/` folder
- ✅ Filename should be your GitHub username with `.txt` extension
- ✅ File should contain only one line: your wallet address
- ✅ Address must start with `0x` followed by 40 hexadecimal characters
- ❌ Do not modify any other files
- ❌ Only one request file per PR

### 4. Submit Pull Request

After creating your file:
1. Commit the file to your branch
2. Open this pull request
3. Wait for validation (automatic, < 10 seconds)
4. Wait for maintainer review and approval

---

## ✅ Checklist

Before submitting, confirm:

- [ ] I created a file in `requests/YOUR_USERNAME.txt`
- [ ] The file contains only my wallet address
- [ ] My address is valid (0x + 40 hex characters)
- [ ] I did not modify any other files
- [ ] This is for Mordor testnet use only
- [ ] I will use these tokens for testing purposes

---

## 🔒 Security & Network

**Network:** Ethereum Classic Mordor Testnet  
**Treasury:** Safe{Core} (Catacomb) 2-of-3 Multisig  
**Platform:** [multisig.etccooperative.org](https://multisig.etccooperative.org/)

---

## ❓ Need Help?

If you encounter issues:
- Check that your file is in the correct location
- Verify your wallet address format
- Review the automatic validation comments
- Ask in the comments below

**Do not include private keys or sensitive information in your request!**