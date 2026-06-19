<div align="center">

# ⛓️ onchain-poc-runner

### *Automated fork-state capture and PoC replay harness.*

[![Python](https://img.shields.io/badge/python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Foundry](https://img.shields.io/badge/Foundry-required-000000?style=flat-square)](https://book.getfoundry.sh/)
[![PoC Templates](https://img.shields.io/badge/templates-8-success?style=flat-square)](#-poc-templates)
[![Forks](https://img.shields.io/badge/forks-5%20chains-blueviolet?style=flat-square)](#-supported-chains)
[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)

</div>

---

## 🎯 The Problem

Writing a PoC is 5% exploit logic and 95% plumbing:
- Capture state before/after
- Calculate fund impact in wei and USD
- Run on multiple chains/forks
- Format the report so a triager reads it
- Generate traces and screenshots

**onchain-poc-runner** automates the 95% so you can focus on the 5%.

---

## ✨ Features

- 🎯 **8 pre-built PoC templates** — covering 90% of DeFi bug types
- 🔄 **Replay harness** — Run any PoC on multiple forks (Ethereum, Arbitrum, Base, Optimism, Ink)
- 📊 **State diff engine** — Capture before/after storage, calculate fund impact in wei and USD
- 📝 **Auto-report generation** — Markdown, PDF, or HTML submission drafts
- 🖼️ **Screenshot capture** — Foundry traces → PNG flow diagrams
- 🤖 **Multi-PoC orchestration** — Run a target's full PoC suite in one command
- 🔗 **GitHub integration** — Auto-push PoC to your writeups repo
- 🎨 **Pretty terminal output** — Live progress, color-coded severity, fund-impact summary

---

## 📦 Installation

```bash
pip install onchain-poc-runner
```

### From source

```bash
git clone https://github.com/AbD02018/onchain-poc-runner
cd onchain-poc-runner
pip install -e .
```

### Requirements

- Python 3.10+
- Foundry (`forge`, `cast`, `anvil`)
- Graphviz (for trace diagrams)

---

## 🚀 Quick Start

```bash
# Initialize a new PoC project from a template
onchain-poc init liquidation-poc --template 01-liquidation

# Edit the generated PoC: write your exploit logic in src/Exploit.t.sol

# Run on a fork
onchain-poc run --fork ethereum --block 19500000

# Run on all forks
onchain-poc run --all-forks --block 19500000

# Generate a submission draft
onchain-poc report --format markdown > submission.md

# Push to your writeups repo
onchain-poc push --repo AbD02018/Bug-Bounty-Writeups-and-PoCs
```

---

## 📚 PoC Templates

| # | Template | Use case |
|---|---|---|
| 01 | **Liquidation** | Seize collateral from underwater positions |
| 02 | **Oracle Manipulation** | Spot oracle + flash loan attack |
| 03 | **Fund Conservation** | Deposit/withdraw loop + rounding check |
| 04 | **Governance Attack** | Flash-loan vote + malicious proposal |
| 05 | **Reentrancy** | Single-fn + cross-fn reentrancy |
| 06 | **Access Control Bypass** | Direct call to admin function |
| 07 | **Signature Replay** | EIP-712 replay across chains/domains |
| 08 | **First Deposit Inflation** | ERC-4626 vault inflation |

> **Need a new template?** Open an issue with the bug class.

---

## 🌐 Supported Chains

| Chain | ID | RPC Env Var |
|---|---|---|
| **Ethereum** | 1 | `ETH_RPC_URL` |
| **Arbitrum One** | 42161 | `ARB_RPC_URL` |
| **Base** | 8453 | `BASE_RPC_URL` |
| **Optimism** | 10 | `OP_RPC_URL` |
| **Ink** | 57073 | `INK_RPC_URL` |

Add your own chain via `~/.config/onchain-poc-runner/chains.toml`.

---

## 🛠️ CLI Reference

```
onchain-poc init <name> [--template N]    Initialize a new PoC project
onchain-poc run [--fork <chain>]          Run PoC on a single fork
onchain-poc run --all-forks                Run PoC on all configured forks
onchain-poc report [--format <fmt>]       Generate submission draft
onchain-poc push --repo <owner/repo>      Push to your writeups repo
onchain-poc diff <before> <after>         Generate state diff
onchain-poc impact <tx-hash> <chain>      Calculate fund impact of a tx
onchain-poc templates list                List all available templates
onchain-poc chains list                   List all configured chains
```

---

## 🤝 Contributing

PRs welcome for:
- New PoC templates
- New chain support
- Better state-diff utilities
- Report-format improvements

See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## 📄 License

MIT — see [LICENSE](LICENSE).

---

<div align="center">
  <sub>Built by <a href="https://github.com/AbD02018">@AbD02018</a> · Smart contract security researcher</sub>
</div>
