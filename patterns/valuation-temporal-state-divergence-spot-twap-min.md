# Pattern: Conservative Selection Asymmetry in Multi-Source Oracles

**Severity**: Critical

**Category**: Temporal State Violation

## Summary
An oracle architecture that applies a globally 'conservative' selection logic (such as minimum value) across both asset and liability valuations, potentially prioritizing transient state fluctuations over stable reference data.

## Consequence
The system may adopt an artificially depressed valuation for obligations, leading to a divergence between recorded state and actual market value, which results in the depletion of system liquidity and internal insolvency.

## Trigger Conditions
- Oracle uses a selection function (e.g., Math.min) to choose between an instantaneous state (spot) and a stabilized state (TWAP).
- The resulting value is used as a denominator or a valuation metric for liabilities/debt.
- The instantaneous state is sourced from a pool whose reserves can be modified within a single atomic execution environment.
- The protocol lacks context-aware pricing logic that distinguishes between collateral-side and debt-side valuation requirements.

## Failure Signature
### On-Chain
- Execution of debt-increasing functions where the valuation is derived from a point-in-time reserve check (getReserves).
- Oracle final price output matches the lower of two sources during a period of high volatility or sudden reserve shifts.
- The price returned by the oracle diverges significantly from the stabilized secondary source (TWAP) within the same block.
### Off-Chain
- External monitor alerts for significant divergence between AMM spot prices and time-weighted average prices for borrowable assets.
- Detection of high-volume borrowing transactions occurring immediately after a sharp, single-block price drop in the underlying pool.

## References
- [Day 1 Report](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/reports/analysis-valuation-temporal-state-divergence-spot-twap-min.md)
- [Invariant](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/invariants/INV-valuation-temporal-state-divergence-spot-twap-min.md)
