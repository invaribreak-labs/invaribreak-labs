# System Analysis

## Summary
When a limit buy order is placed, the portfolio locks the full quote amount (limitPrice × quantity). If the order is matched at an execution price lower than the limit price, the matching contract transfers only the execution amount from the locked collateral and does not adjust the remaining locked portion, leaving a surplus locked in the portfolio.

## Threat Model
- **threat_source**: Any market participant capable of submitting a matching order at a price lower than the limit price.
- **threat_boundary**: The execution path between order matching and portfolio balance adjustment.

## Assumptions
- The portfolio contract locks the full quote amount for each limit buy order before matching.
- The matching engine can select an execution price that is lower than the limit price.
- Portfolio.addExecution deducts only the execution amount from the maker's locked collateral.
- No automatic release of the surplus portion of locked collateral occurs within addExecution.
- The system operates on a deterministic state machine without external state resets during the matching flow.

## System Model
### Components
- Matching Engine
- Portfolio Manager
- Limit Order Book
- Token Transfer Module
- Execution Scheduler

## Break Point
> **Intended State**: LockedCollateral[maker] = 0
>
> **Actual State**: LockedCollateral[maker] = SurplusAmount
>
> **Divergence**: ExecutionPrice < LimitPrice

