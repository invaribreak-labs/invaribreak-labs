# CRITICAL: Permanent Fund Locking for Makers when Limit Buy Orders are filled at a better price (Missing Surplus Refund)

**Summary**: When a limit buy order is placed, the Portfolio contract locks the full quote amount (limit price × quantity). If the order is matched at a lower execution price, TradePairs calls Portfolio.addExecution with the execution price, which deducts only the executed amount. No subsequent routine releases the surplus portion of the originally locked collateral, leaving funds permanently locked after the order is marked filled.

## Failure Mechanism
After Portfolio.addExecution completes, the system does not invoke any routine to adjust the maker's remaining locked collateral when the execution price is lower than the limit price. This creates a state divergence where the locked balance exceeds the sum of active orders, resulting in surplus funds remaining locked indefinitely.

## Component Analysis
- TradePairs contract – orchestrates order matching and invokes collateral adjustments.
- Portfolio contract – manages locked and available balances for each trader.
- Limit order objects – contain price, quantity, side, and trader address.
- Matching engine – determines execution price and quantity for order pairs.
- Ethereum Virtual Machine – execution environment for the smart contracts.

## Impact
- **direct**: The maker's surplus collateral (e.g., 100 USDT) stays locked in the Portfolio contract despite having no open orders, rendering those funds inaccessible to the maker.
- **indirect**: Erosion of user trust in the platform, potential reduction in liquidity provision, and possible regulatory scrutiny due to funds being effectively frozen.

## Fix
Introduce a surplus‑refund step after the addExecution call in the standard matchOrder flow. The logic should mirror the auction‑mode handling: compute the difference between the limit price and the execution price, then call Portfolio.adjustAvailable with INCREASEAVAIL to release the surplus back to the maker's available balance.

## Associated Failure Knowledge
- **Report**: [Failure Analysis Report](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/reports/analysis-collateral-state-divergence-missing-refund.md)
- **Invariant**: [Invariant Definition](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/invariants/INV-collateral-state-divergence-missing-refund.md)
- **Pattern**: [Failure Pattern](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/patterns/collateral-state-divergence-missing-refund.md)
- **Exploit Path**: [Exploit Path](https://github.com/invaribreak-labs/invaribreak-labs/blob/main/exploit-paths/collateral-state-divergence-missing-refund.md)
