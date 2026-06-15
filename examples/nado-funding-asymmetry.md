# Example PoC — Nado Funding Asymmetry

This is a real PoC that was used to validate a finding (which turned out to be a Dead Lead, not a bug). It's preserved here as a working example of the PoC pattern.

## File Structure

```
nado-funding-poc/
├── foundry.toml
├── src/
│   └── NadoPoCHelper.sol
├── test/
│   └── NadoFundingPoC.t.sol
└── reports/
    └── validation.md
```

## Key Components

### `src/NadoPoCHelper.sol`
- Read-only helper that exposes `getSettlementState(productId)` for the protocol
- Uses `getSettlementState` to read `cumulativeFundingLongX18` / `cumulativeFundingShortX18`

### `test/NadoFundingPoC.t.sol`
- Forks Ink mainnet at block 47,998,473
- Seeds positions in product 2 (BTC perp)
- Calls `updateStates` to advance funding
- Asserts that `longDelta == shortDelta` (the original "anomaly" claim)
- **Result: 2/2 tests passed, but the "asymmetry" was confirmed to be CORRECT math**

## Lessons

The PoC worked — it captured the state change accurately. But the validation discipline showed:
- Code is self-consistent
- Same-direction `+= paymentAmount` on both accumulators is the **correct convention** given how `_updateBalance` uses `balance.amount` sign
- If we "fixed" it (e.g., flipped short to `-=`), we'd INTRODUCE the actual bug

This is why **PoC demonstrating state change ≠ bug**. The validation discipline requires proof of **economic impact**, not just pattern matching.
