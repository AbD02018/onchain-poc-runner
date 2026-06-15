# ⛓️ onchain-poc-runner

> **Automated fork-state capture and PoC replay harness for smart contract bug reports.**

The harness you wish you had when you were writing your first HackenProof submission. Automates the boring parts of PoC development: state capture, replay, fund impact calculation, and report generation.

---

## ✨ Features

- 🎯 **Pre-built PoC templates** — 8 patterns covering 90% of DeFi bug types
- 🔄 **Replay harness** — Run any PoC on multiple forks (Ethereum, Arbitrum, Base, Optimism, Ink)
- 📊 **State diff engine** — Capture before/after storage, calculate fund impact in wei and USD
- 📝 **Auto-report generation** — Markdown, PDF, or HTML submission drafts
- 🖼️ **Screenshot capture** — Foundry traces → PNG flow diagrams
- 🤖 **Multi-PoC orchestration** — Run a target's full PoC suite in one command
- 🔗 **GitHub integration** — Auto-push PoC to your writeups repo

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

- Python 3.8+
- Foundry (`forge`, `cast`, `anvil`)
- `requests`, `jinja2`, `web3`, `pyyaml`

---

## 🚀 Usage

### Initialize a new PoC

```bash
onchain-poc init --name my-poc --target NadoProtocol --type fund-conservation
```

This creates:
```
my-poc/
├── src/
│   └── MyPoC.sol
├── test/
│   └── MyPoC.t.sol
├── poc.yaml
└── reports/
```

### Run PoC

```bash
onchain-poc run --rpc $MAINNET_RPC --block 19000000
```

### Generate report

```bash
onchain-poc report --format markdown --output report.md
```

### Multi-chain replay

```bash
onchain-poc run --chain mainnet,arbitrum,base --block latest
```

---

## 📂 PoC Templates

| Template | Use For |
|---|---|
| `fund-conservation` | Drain, balance miscalculation |
| `liquidation-bound` | Liquidation gives attacker more than owed |
| `oracle-staleness` | Stale price allows bad debt |
| `signature-forgery` | EIP-712 replay, ecrecover misuse |
| `reentrancy` | Cross-function reentrancy |
| `access-control` | Function permission bypass |
| `fee-evasion` | Bypassing protocol fees |
| `governance` | Vote manipulation, quorum bypass |

---

## 📖 Example

```bash
$ onchain-poc init --name nado-fund-drain --target NadoProtocol --type fund-conservation
[+] Created PoC structure at ./nado-fund-drain
[+] Loaded template: fund-conservation
[+] Edit src/NadoFundDrain.sol and test/NadoFundDrain.t.sol

$ cd nado-fund-drain
# (edit the PoC to replicate the bug)

$ onchain-poc run --rpc $MAINNET_RPC --block 47998473
[+] Forking mainnet at block 47,998,473
[+] Deploying attacker contract
[+] Running exploit
[+] Capturing state diff...

=== State Diff ===
attacker.balance  : 0.0 ETH -> 127.3 ETH (+127.3 ETH)
protocol.balance   : 1,247.5 ETH -> 1,120.2 ETH (-127.3 ETH)
LP_token.totalSupply : 1,000,000 -> 1,000,000 (no change)

[+] Impact: +127.3 ETH (~$382,000 USD at $3000/ETH)
[+] PoC PASSED — economic impact proven

$ onchain-poc report --format markdown
[+] Report generated: report.md
```

---

## 🏗️ Architecture

```
onchain-poc-runner/
├── onchain_poc/
│   ├── cli.py                 # Click-based CLI
│   ├── core/
│   │   ├── runner.py          # PoC orchestrator
│   │   ├── state_diff.py      # Before/after capture
│   │   ├── impact.py          # Fund impact in wei/USD
│   │   └── report.py          # Markdown/PDF/HTML generation
│   ├── templates/
│   │   ├── fund_conservation/
│   │   ├── liquidation_bound/
│   │   └── ... (8 total)
│   └── integrations/
│       ├── foundry.py         # forge wrapper
│       ├── etherscan.py       # Contract source fetcher
│       ├── coingecko.py       # Price feed
│       └── github.py          # Auto-push
├── examples/
│   ├── nado-fund-drain/
│   └── aave-liquidation/
└── pyproject.toml
```

---

## 📄 License

MIT — see [LICENSE](LICENSE)

---

## 🤝 Contributing

1. Add a new PoC template to `onchain_poc/templates/`
2. Add a real-world example to `examples/`
3. Submit PR with bug bounty submission that used the template

---

<div align="center">

*Capture state. Replicate. Calculate impact. Submit.*

</div>
