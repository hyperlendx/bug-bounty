# HyperLend Bug Bounty

---

Bug bounty rewards will be paid in USDC and vested HPL for responsible disclosure of bugs based on their severity.

### Scope

HyperLend bug bounty covers smart contracts available at (https://docs.hyperlend.finance/developer-documentation/contract-addresses).

Update (19/5/2025): DDoS attacks on the UI/API/RPCs are not in-scope!

Update (13/3/2026): If you are submitting AI-generated reports, please at least verify that the contracts you are reporting are deployed on mainnet (see the scope above) and not old deprecated code on github.

### Contact

Please send reports to: `fbslo@hyperlend.finance`

---

Known commonly reported issues:
- AaveOracle.sol (in hyperlend-core) doesn't validate staleness and uses latestAnswer() directly.
- USDhl oracle uses getPriceUnsafe - since USDhl is a stablecoin, using a (potentially) stale price is preferable to reverting transactions. USDhl market is also deprecated.
- LiquidSwap adapter doesn't have any authentication, and leftover balances can be taken by anyone - are never used directly/independently; they are always wrapped inside a multicall, so they should never hold any balances at the end of the transaction.
- USR price is hardcoded - the previously used Chainlink oracle was shut down, and $0.15 was the price at that time; and since there are no other safe oracles that we could use, we decided hardcoding the price is the best solution. USR market is also deprecated.
- wHLP price oracle can serve a stale NAV as fresh - since there is no other "real-time" wHLP oracle, we rely on the accountant's value.
- If both primary and secondary are unhealthy (or primary reverts) in DualFallbackOracle, it returns the newer data (even if it's unhealthy).
- Any economic attack that relies on a significant price change, e.g., a fast crash causing bad debt.
