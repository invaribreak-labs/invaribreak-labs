# Pattern: Collateral Surplus Retention on Better-Price Execution

**Severity**: Critical

**Category**: Accounting State Inconsistency

## Summary
An accounting failure occurs when a system locks collateral based on a maximum potential obligation but only deducts the actual execution amount during a settlement event, failing to release the surplus back to the user's available balance.

## Consequence
The system enters a state of permanent collateral locking where a user's locked balance remains non-zero despite having no active obligations or open orders. This creates a divergence between the total tracked collateral and the sum of active liabilities.

## Trigger Conditions
- The system uses a pessimistic locking mechanism that secures the maximum possible quote amount at the time of order creation.
- The execution engine supports matching at a price more favorable than the original limit price.
- The settlement routine handles the transfer of execution funds but lacks a reconciliation step to adjust the remaining locked collateral.
- The order lifecycle transitions to a 'completed' or 'filled' state, removing the record of the original obligation while the surplus remains in a locked state.

## Failure Signature
### On-Chain
- Total locked collateral for an account is greater than the sum of (limit price * remaining quantity) for all active orders.
- A 'Filled' status is assigned to an order without a corresponding 'Release' or 'Refund' event for the surplus collateral.
- The sum of all traders' locked balances in the accounting contract exceeds the total value required to cover the current order book.
### Off-Chain
- Audit logs show execution events where 'execution price' < 'order limit price' without subsequent portfolio adjustment calls.
- Dashboard monitoring indicates accounts with zero active orders but positive 'locked' balance values.

## References
- [Day 1 Report](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/reports/analysis-collateral-state-inconsistency-better-price-fill.md)
- [Invariant](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/invariants/INV-collateral-state-inconsistency-better-price-fill.md)
