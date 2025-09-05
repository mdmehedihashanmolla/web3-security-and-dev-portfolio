<div align="center">
  <h1>🛡️ Web3 Security & Development Portfolio</h1>
  <p><strong> Web3 Security Researcher & Developer</strong></p>
  <p><em>Building the future by breaking what's broken.</em></p>
  
  <img src="https://img.shields.io/badge/Competitive Audits-blueviolet?style=flat-square&logo=shield" alt="Competitive Audits" />
  <img src="https://img.shields.io/badge/Shadow Audits-gray?style=flat-square&logo=code" alt="Shadow Audits" />
  <img src="https://img.shields.io/badge/CTF Solutions-orange?style=flat-square&logo=terminal" alt="CTF Solutions" />
  <img src="https://img.shields.io/badge/Dev Projects-green?style=flat-square&logo=ethereum" alt="Development Projects" />
  <img src="https://img.shields.io/badge/Vulnerability Research-red?style=flat-square&logo=bug" alt="Vulnerability Research" />
  <img src="https://img.shields.io/badge/Smart_Contracts-9cf?style=flat-square&logo=blockchain-dot-com&logoColor=black" alt="Smart Contract Projects" />

</div>

## 📋 Overview

A showcase of real-world work as a **Web3 Security Researcher** and **Developer**.

## 🚀 Highlights

- 🛡️ Competitive audit reports 
- 💥 Vulnerability research with PoCs and mitigation guides
- 📊 Real-world hack replications with technical breakdowns
- 🔧 Secure smart contracts and developer tooling
- 🧱 Protocol-level projects (vaults, lending, staking systems)
- 🌐 Full-stack dApps (frontend-integrated Web3 apps)


<div align="center">

## 📚 Table of Contents

| Section | Link |
|--------|------|
| 🏆 Competitive Audits | [Link](#-competitive-audits) |
| 🔍 Shadow Audits | [Link](#-shadow-audits) |
| 🐛 Vulnerability Research | [Link](#-vulnerability-research) |
| 💻 Smart Contract Projects | [Link](#-smart-contract-projects) |
| ⚡  Exploit Replications | [Link](#-exploit-replications) |
| 🌐 Full-Stack dApp Projects  | [Link](#-full-stack-dapp-projects) |
| 🎯 CTF Solutions | [Link](#-ctf-solutions) |
| 🤝 Connect & Collaborate | [Link](#-connect--collaborate) |
| 📜 Disclaimer | [Link](#️-disclaimer) |

</div>


---

## 🏆 Competitive Audits

*Professional audit contest submissions and rankings*

| Contest Platform | Protocol | Language  | Date     | Report |
|------------------|----------|-----------|----------|--------|
| **Code4rena**    | [Vault Protocol (Demo Structure)](./audits/code4rena/vault-protocol/) | Solidity  | Jul 2025 | [📄 PDF](./audits/code4rena/vault-protocol/findings.md) |


### Key Achievements  *(Demo Format)*
- 🥇 **Top 10%** ranking in Code4rena contests
- 🎯 **High-severity** findings in production protocols
- 💰 **$X,XXX** in total bounties earned

**[📁 View All Audit Reports →](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/tree/main/01-Competitive-Audits)**

---

## 🔍 Shadow Audits

*Independent security reviews of major protocols*

| Protocol       | Type        | Focus Area               | Language | Date       | Report     |
|----------------|-------------|---------------------------|----------|------------|------------|
| [**Uniswap V3**](./shadow-audits/uniswap-v3/) | DEX (Demo Structure) | Concentrated Liquidity | Solidity | March 2025 | [📋 PDF](./shadow-audits/uniswap-v3/security-analysis.md) |


**[📁 View All Shadow Audits →](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/tree/main/02-Shadow-Audits)**

---

## 🐛 Vulnerability Research

*Original vulnerability research and proof-of-concepts*

| Vulnerability Type   | Theory / Mitigation Guide | Test Suite   |
|----------------------|---------------------------|--------------|
| [**1. Single Function Reentrancy**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/reentrancy/1_SingleFunction.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/reentrancy/Readme.md#1-single-function-reentrancy) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/reentrancy/1_SingleFunction.t.sol) |
| [**2. Cross-Function Reentrancy**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/reentrancy/2_CrossFunction.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/reentrancy/Readme.md#2-cross-function-reentrancy) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/reentrancy/2_CrossFunction.t.sol) |
| [**3. tx-origin-phishing**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/access_control/TxOrigin.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/access_control/Readme.md#1-phishing-with-txorigin) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/access_control/TxOrigin.t.sol) |
| [**4. WeakAccessControl**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/access_control/WeakAccessControl.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/access_control/Readme.md#2-weak--missing-access-control) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/access_control/WeakAccessControl.t.sol) |
| [**5. Improper Function Visibility**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/access_control/FunctionVisibility.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/access_control/Readme.md#3-function-visibility-issues) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/access_control/FunctionVisibility.t.sol) |
| [**6. Integer Overflow/Underflow**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/arith_logic_errors/IntegerVulnerability.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/arith_logic_errors/Readme.md#1-integer-overflowunderflow) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/arith_logic_errors/IntegerVulnerability.t.sol) |
| [**7. Rounding Errors**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/arith_logic_errors/RoundingError.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/arith_logic_errors/Readme.md#2-rounding-errors---precision-loss-in-calculations) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/arith_logic_errors/RoundingError.t.sol) |
| [**8. Off-by-One Errors**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/arith_logic_errors/Staking.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/arith_logic_errors/Readme.md#3-off-by-one-errors---boundary-condition-mistakes) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/arith_logic_errors/Staking.t.sol) |
| [**9. Gas Griefing**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/DOS/Gas_griefing.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/DOS/Readme.md#1-gas-griefing---intentional-gas-consumption) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/DOS/Gas_griefing.t.sol) |
| [**10. Loop Attacks**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/DOS/Loop_attack.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/DOS/Readme.md#2-loop-attacks---unbounded-iterations) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/DOS/Loop_attack.t.sol) |
| [**11. DoS with Revert**](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/DOS/DoSWithRevert.sol) | [📖 Theory / Mitigation ](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/src/DOS/Readme.md#3-dos-with-revert---blocking-execution-with-reverts) | [🧪 PoC](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/blob/main/03-Vulnerability-Research/vulnerability/test/DOS/DoSWithRevert.t.sol) |


**[📁 View All Vulnerability Research →](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/tree/main/03-Vulnerability-Research)**

---

## 💻 Smart Contract Projects

*Production-ready smart contracts with comprehensive security measures*

| Project Name | Type | Features | Language | Date | 
|--------------|------|----------|----------|------|
| [**DeFi Vault Protocol**](./smart-contracts/defi-vault/) | DeFi Vault (Demo Structure) | Auto-compounding, Flash Loan Protection | Solidity | Aug 2025 | 

**[📁 View All Smart Contract Projects →](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/tree/main/04-Smart-Contract-Projects)**

---

## ⚡ Exploit Replications

*Historical DeFi hacks recreated for educational purposes*

| Incident | Date | Loss (USD) | Vulnerability | PoC |
|----------|------|------------|---------------|-----|
| [**Euler Finance**](./exploits/euler-2023/) | Mar 2023 | $196M | Donation Attack | [🧪 Test](./exploits/euler-2023/test/EulerExploit.t.sol) |

**[📁 View All Exploit Replications →](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/tree/main/05-Exploit-Replications)**

---

## 🌐 Full-Stack dApp Projects 

*Full-stack decentralized applications demonstrating end-to-end Web3 development.*

| Project | Type | Tech Stack | Live |
|---------|------|------------|------|
| [**Gas-Optimized Vault**](./projects/gas-vault/) | DeFi Vault | Solidity, Foundry, Next.js, Wagmi | ✅ [Link](https://vault.example.xyz) |
| [**NFT Marketplace**](./projects/nft-marketplace/) | NFT Platform | Solidity, Hardhat, React, IPFS | ❌ Not Deployed |

**[📁 View All Development Projects →](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/tree/main/06-Full-Stack-dApp-Projects%20)**

---

## 🎯 CTF Solutions

### Ethernaut Solutions
| Challenge | Topic | Solution |
|-----------|-------|----------|
| [Fallback](./ctf/ethernaut/01-fallback/) | Access Control | [💻 Code](./ctf/ethernaut/01-fallback/Exploit.sol) |

### Damn Vulnerable DeFi Solutions
| Challenge | Topic | Solution |
|-----------|-------|----------|
| [Unstoppable](./ctf/dvd/01-unstoppable/) | Flash Loans | [💻 Code](./ctf/dvd/01-unstoppable/Exploit.sol) |

### Capture The Ether Solutions
| Challenge | Topic | Solution |
|-----------|-------|----------|
| [Deploy a contract](./ctf/cte/01-deploy/) | Contract Deployment | [💻 Code](./ctf/cte/01-deploy/Solution.sol) |


**[📁 View All CTF Solutions →](https://github.com/mdmehedihashanmolla/web3-security-and-dev-portfolio/tree/main/07-CTF-Solutions)**

---

# 🌐 Connect & Collaborate  

### 🔗 **Professional Profiles**  
| Platform       | Link                                                                 |
|----------------|----------------------------------------------------------------------|
| 🐦 **Twitter**  | [@0xMehediSec](https://x.com/0xMehediSec)                           |
| 💼 **LinkedIn** | [MD Mehedi Hashan Molla] (Inactive)                                 |
| 📧 **Email**    | `mdmehedihashanmolla@gmail.com`                                     |
| ✍️ **Blog**     | [Medium Articles](https://medium.com/@mdmehedihashanmolla)          |



### 🚀 **Open To**  
- **🛡️ Web3 Audits**  
  → DeFi protocols | L2 solutions | Novel architectures  
- **👨‍💻 Development**  
  → Smart Contracts | Full-Stack dApps | Security Tooling  
- **🔬 Research**  
  → Novel attack vectors | Gas optimization | Protocol analysis  
- **🎤 Engagement**  
  → Conference Talks | Mentorship | Technical Writing  

---  

# ⚠️ Disclaimer  

**For Educational Use Only**  
All code, writeups, and exploits in this repository are for **educational purposes only**.  
Do not use this content for unauthorized or malicious activities.

