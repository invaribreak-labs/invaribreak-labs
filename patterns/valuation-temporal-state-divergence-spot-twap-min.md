# Pattern: Spot‑TWAP Minimum Selection Temporal Divergence

**Severity**: Critical

**Category**: Temporal State Violation

## Summary
An oracle that computes a debt valuation price by taking the minimum of an instantaneous spot price from an AMM pool and a time‑weighted average price (TWAP) can produce a transiently low price when the spot price is temporarily depressed.

## Consequence
The borrowing contract may consider a loan adequately collateralized while the true market value of the debt remains high, resulting in protocol under‑collateralization and potential depletion of liquidity.

## Trigger Conditions
- The oracle combines a spot price obtained from an AMM pool with a TWAP using a min() operation for debt valuation.
- The spot price can be altered within a single transaction (e.g., by a large trade that changes pool reserves).
- The altered spot price becomes lower than the TWAP at the moment of price computation.
- A borrowing operation queries the oracle and uses the returned price to assess collateral adequacy.

## Failure Signature
### On-Chain
- Oracle price for an asset drops sharply within one block while the TWAP remains stable.
- Borrowing contract emits an event approving a loan amount that is large relative to the collateral after the price drop.
- Price selection logic logs indicate the use of the min() function for the final price.
### Off-Chain
- Monitoring systems detect a significant divergence between spot price feeds and TWAP feeds for the same asset.

## References
- [Day 1 Report](../reports/analysis-valuation-temporal-state-divergence-spot-twap-min.md)
- [Invariant](../invariants/INV-valuation-temporal-state-divergence-spot-twap-min.md)
