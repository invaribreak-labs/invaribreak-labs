# Pattern: Locked Collateral Residual Retention

**Category**: State Invariant Violation, Resource Accounting Error

## Consequence
Persistent mismatch between recorded locked collateral and actual order obligations, leading to state inconsistency

## Trigger Conditions
- A limit order creates a locked collateral entry equal to limit_price × quantity
- The order is matched at an execution price lower than the limit price
- The execution routine transfers only the execution amount and does not invoke a surplus release operation

## Failure Signature
### On-Chain
- Locked collateral value for the maker remains non‑zero after the order is marked filled
- Absence of a surplus refund event in transaction logs
### Off-Chain
- Discrepancy between the maker's reported locked collateral and the sum of active orders in external order‑book snapshots

## References
- [Day 1 Report](../reports/analysis-collateral-state-inconsistency-better-price-fill.md)
- [Invariant](../invariants/INV-collateral-state-inconsistency-better-price-fill.md)
