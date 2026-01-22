# Invariant: INV-BASEORACLE-VAL-LOWER-BOUND

## Formal Statement
`∀ a ∈ Assets : price_debt(a) ≥ twapPrice(a)`

## English Explanation
For every borrowable asset, the USD value used to assess the debt must never be lower than the TWAP price supplied by the external aggregator. This guarantees that a transient drop in the spot price cannot reduce the reported value of the borrowed asset.

## Variables
- **price_debt(a)**: USD valuation used by the borrowing contract for the debt of asset a
- **twapPrice(a)**: Time‑weighted average price of asset a obtained from the TWAP aggregator
- **a**: Identifier of a borrowable asset

## Safety Property Type
Valuation

## Counterexample State
> An external user obtains a flash loan of asset X, swaps a large amount of X into the AMM pool used by the oracle, causing the spot price of X to fall dramatically (e.g., to $0.01) while the TWAP price remains near $100. The user then calls the borrowing function, which queries the oracle. The oracle returns the low spot price as the valuation for the debt (price_debt = $0.01). In this state, price_debt < twapPrice, violating the invariant.

## References
- [Day 1 Report](../reports/analysis-valuation-temporal-state-divergence-spot-twap-min.md)
