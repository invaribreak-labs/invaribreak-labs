# Pattern: Zero-Balance Repay Preference Abort

**Severity**: High

**Category**: Temporal State Divergence

## Summary
When a liquidation preference includes a repayment for a token with zero on‑chain balance, the contract returns an error that aborts the entire liquidation transaction.

## Consequence
The liquidation process halts, leaving the under‑collateralized account unchanged and preventing any further liquidation steps, which can lead to protocol insolvency.

## Trigger Conditions
- A liquidation preference list contains a Repay action for a specific token.
- The account's balance for that token is zero at the time of liquidation.
- The liquidation engine processes preferences sequentially and propagates errors via the ? operator.

## Failure Signature
### On-Chain
- Transaction reverts with error ZeroDebtTokens (or similar) during liquidation.
- No state changes to the account or collateral after a failed liquidation attempt.
- Presence of a Repay preference for a token with zero balance in the account's stored preference queue.
### Off-Chain
- Monitoring alerts indicating repeated liquidation failures for the same account.

## References
- [Day 1 Report](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/reports/analysis-account-temporal-state-divergence-zero-balance-repay.md)
- [Invariant](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/invariants/INV-account-temporal-state-divergence-zero-balance-repay.md)
