```json
{
  "naming_slug": "collateral-state-divergence-missing-refund",
  "summary": "The TradePairs contract matches limit buy orders against sell orders. When a buy order is filled at a price lower than the limit price, the contract transfers the execution amount from the maker's locked collateral but does not adjust the remaining locked portion. Consequently, the maker's portfolio retains a surplus of locked collateral after the order is marked filled.",
  "threat_model": {
    "threat_source": "Any market participant capable of submitting matching orders that trigger the standard matchOrder flow.",
    "threat_boundary": "The execution path of TradePairs.matchOrder that interacts with Portfolio.addExecution."
  },
  "assumptions": [
    "The contracts are deployed on the Ethereum Virtual Machine and written in Solidity.",
    "A limit buy order locks the full quote amount (limitPrice × quantity) in the Portfolio contract before matching.",
    "Portfolio.addExecution deducts only the execution price amount from the maker's locked collateral and transfers it to the taker.",
    "No automatic adjustment of the remaining locked collateral occurs within addExecution.",
    "The execution environment permits the matchOrder function to be called repeatedly for different order pairs."
  ],
  "system_model": {
    "components": [
      "TradePairs contract that orchestrates order matching.",
      "Portfolio contract that manages locked and available balances for each trader.",
      "Limit order objects containing price, quantity, side, and trader address.",
      "Matching engine that determines execution price and quantity.",
      "Ethereum execution environment."
    ],
    "data_flow": [
      "1. Maker places a limit buy order; Portfolio locks limitPrice × quantity as collateral.",
      "2. Matching engine finds a sell order with a lower execution price.",
      "3. TradePairs calls Portfolio.addExecution with the execution price and quantity.",
      "4. Portfolio transfers the execution amount from the maker's locked collateral to the taker.",
      "5. Order is marked filled and removed, but the surplus portion of the originally locked collateral remains locked."
    ]
  },
  "break_point": {
    "location": "State divergence occurs after Portfolio.addExecution completes but before any surplus adjustment logic is applied.",
    "behavior": "The maker's locked collateral retains a surplus amount that no longer corresponds to any active order, creating a mismatch between locked balance and actual obligations."
  }
}
```