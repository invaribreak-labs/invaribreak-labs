# Pattern: Collateral Surplus Retention on Better‑Price Fill

**Severity**: Critical

**Category**: Temporal State Violation

## Summary
When a limit order is executed at a price better than the limit price, the matching logic deducts only the execution amount from the locked collateral and fails to release the surplus, leaving residual locked balance for the maker.

## Consequence
The maker ends up with a non‑zero locked balance while having no open orders, causing the surplus funds to remain permanently inaccessible.

## Trigger Conditions
- A limit order locks the full quote‑amount based on its limit price before matching.
- The order is matched at an execution price lower than the limit price.
- The matching function calls the portfolio's execution routine without invoking any surplus‑refund logic.
- The portfolio's execution routine deducts only the execution amount from the locked collateral.

## Failure Signature
### On-Chain
- Locked collateral for a trader remains > 0 after the order is marked filled and removed from the order book.
- Invariant violation: locked collateral ≠ sum of (price × remaining quantity) of all open orders for the trader.
### Off-Chain
- Monitoring dashboards report a mismatch between reported locked balances and the list of active orders.
- Audit logs show order fill events without corresponding collateral‑release events.

## References
- [Day 1 Report](../reports/analysis-collateral-state-inconsistency-better-price-fill.md)
- [Invariant](../invariants/INV-collateral-state-inconsistency-better-price-fill.md)
