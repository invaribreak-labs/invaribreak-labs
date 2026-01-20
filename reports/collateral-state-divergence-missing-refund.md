# CRITICAL: Permanent Fund Locking for Makers when Limit Buy Orders are filled at a better price (Missing Surplus Refund)

**Summary**: When a limit buy order is matched at a price lower than the limit price, the TradePairs contract deducts only the execution amount from the maker's locked collateral. The remaining surplus remains locked after the order is marked filled, creating a state inconsistency.

## Failure Mechanism
After Portfolio.addExecution transfers the execution amount, the contract does not invoke any routine to release the surplus of the originally locked collateral. Consequently, the maker's locked balance exceeds the sum of their active orders, leaving funds permanently inaccessible.

## Component Analysis
- TradePairs.sol – orchestrates order matching and invokes Portfolio functions.
- Portfolio.sol – manages locked and available balances for each trader.
- Limit order objects – contain price, quantity, side, and trader address.
- Matching engine – determines execution price and quantity for order pairs.
- Ethereum Virtual Machine – execution environment for the contracts.

## Impact
- **direct**: A maker's surplus collateral (e.g., 100 USDT) remains locked in the Portfolio contract despite having no open orders, effectively rendering those funds unusable.
- **indirect**: Erosion of user trust, reduced willingness to provide liquidity, and potential regulatory scrutiny due to systematic fund immobilization.

## Fix
Introduce a surplus‑release step after the addExecution call in the standard matchOrder path. The logic should mirror the auction‑mode refund: compute the difference between the limit price and the execution price, then call Portfolio.adjustAvailable with INCREASEAVAIL to return the surplus to the maker's available balance.

## Associated Failure Knowledge
- **Report**: [Failure Analysis Report](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/reports/analysis-collateral-state-divergence-missing-refund.md)
- **Invariant**: [Invariant Definition](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/invariants/INV-collateral-state-divergence-missing-refund.md)
- **Pattern**: [Failure Pattern](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/patterns/collateral-state-divergence-missing-refund.md)
- **Exploit Path**: [Exploit Path](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/exploit-paths/collateral-state-divergence-missing-refund.md)
