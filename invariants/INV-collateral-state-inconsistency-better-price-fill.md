# Invariant: INV-COLLATERAL-CONSISTENCY-LOCKED-ORDER

## Formal Statement
`∀ t ∈ Traders : Portfolio.locked[t] = Σ_{o ∈ OpenOrders(t)} (o.price × o.quantity)`

## English Explanation
For every trader, the amount of collateral that the Portfolio contract reports as locked must always be exactly the sum of (limit price × remaining quantity) of all that trader's active limit orders. When an order is completely filled, its contribution to the sum becomes zero, and the locked collateral must be released accordingly.

## Variables
- **t**: Identifier of a trader (maker) in the system
- **Portfolio.locked[t]**: Total amount of quote token currently locked for trader t in the Portfolio contract
- **OpenOrders(t)**: Set of limit orders that are still open (not filled or cancelled) for trader t
- **o.price**: Limit price specified in order o (quote token per base token)
- **o.quantity**: Remaining quantity of base token to be filled in order o

## Safety Property Type
Conservation

## Counterexample State
> Trader A creates a limit buy order O with price = 10 USDT and quantity = 100 units. Portfolio.locked[A] becomes 1000 USDT. A matching sell order executes at price = 9 USDT, fully filling O. The system marks O as filled and removes it from OpenOrders(A) but fails to release the surplus 100 USDT. After execution, Portfolio.locked[A] = 100 USDT while Σ_{o ∈ OpenOrders(A)} (o.price × o.quantity) = 0, violating the invariant. This state is reachable by any market participant who submits a sell order at a better price through the public matching interface.

## References
- [Day 1 Report](../reports/analysis-collateral-state-inconsistency-better-price-fill.md)
