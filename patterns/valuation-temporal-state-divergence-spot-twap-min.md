# Pattern: Temporal Price Divergence via Minimum Selection

**Category**: Temporal State Violation, Oracle Pricing Inconsistency

## Consequence
State inconsistency leading to under‑collateralized debt positions

## Trigger Conditions
- Oracle price computation selects the minimum of an instantaneous spot price and a time‑weighted average price for debt valuation
- Spot price source can be altered within the same block execution
- Borrowing logic relies on the selected price to assess debt collateralization

## Failure Signature
### On-Chain
- Debt valuation price returned by the oracle equals the spot price when spot < TWAP
- Borrowing transaction records a debt amount whose computed USD value is lower than the TWAP at that block
### Off-Chain
- Observed divergence between spot price and TWAP at the time of borrowing
- Analytics detect debt positions with valuation below the TWAP reference

## References
- [Day 1 Report](../reports/analysis-valuation-temporal-state-divergence-spot-twap-min.md)
- [Invariant](../invariants/INV-valuation-temporal-state-divergence-spot-twap-min.md)
