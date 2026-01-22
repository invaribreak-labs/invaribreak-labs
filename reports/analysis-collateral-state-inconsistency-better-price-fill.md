```json
{
  "naming_slug": "collateral-state-inconsistency-better-price-fill",
  "summary": "When a limit buy order is placed, the portfolio locks the full quote amount (limitPrice × quantity). If the order is matched at an execution price lower than the limit price, the matching contract transfers only the execution amount from the locked collateral and does not adjust the remaining locked portion, leaving a surplus locked in the portfolio.",
  "threat_model": {
    "threat_source": "Any market participant capable of submitting a matching order at a price lower than the limit price.",
    "threat_boundary": "The execution path between order matching and portfolio balance adjustment."
  },
  "assumptions": [
    "The portfolio contract locks the full quote amount for each limit buy order before matching.",
    "The matching engine can select an execution price that is lower than the limit price.",
    "Portfolio.addExecution deducts only the execution amount from the maker's locked collateral.",
    "No automatic release of the surplus portion of locked collateral occurs within addExecution.",
    "The system operates on a deterministic state machine without external state resets during the matching flow."
  ],
  "system_model": {
    "components": [
      "Matching Engine",
      "Portfolio Manager",
      "Limit Order Book",
      "Token Transfer Module",
      "Execution Scheduler"
    ],
    "data_flow": [
      "1. Maker places a limit buy order; Portfolio locks limitPrice × quantity as collateral.",
      "2. Matching Engine identifies a sell order with a lower execution price.",
      "3. Matching Engine invokes Portfolio.addExecution with execution price and quantity.",
      "4. Portfolio transfers executionAmount from the maker's locked collateral to the taker.",
      "5. Order is marked filled; no further adjustment to the remaining locked collateral occurs."
    ]
  },
  "break_point": {
    "location": "Portfolio.addExecution → Locked Collateral Update",
    "behavior": "After execution, the locked collateral retains a surplus amount that no longer corresponds to any active order."
  },
  "break_point_formal": {
    "intended_state": "LockedCollateral[maker] = 0",
    "actual_state": "LockedCollateral[maker] = SurplusAmount",
    "divergence_condition": "ExecutionPrice < LimitPrice"
  }
}
```