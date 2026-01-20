```json
{
  "invariant_id": "INV-BASEORACLE-PRICE-VAL-001",
  "formal_statement": "∀ asset ∈ BorrowableAssets, let spotPrice = Oracle.spotPrice(asset), twapPrice = Oracle.twapPrice(asset), finalPrice = max(spotPrice, twapPrice). Then for any borrow transaction with amount b and collateral value c, the protocol must enforce b * finalPrice ≤ c.",
  "variables": {
    "spotPrice": "Instantaneous price obtained from AMM reserves",
    "twapPrice": "Time‑weighted average price supplied by the TWAP aggregator",
    "finalPrice": "Price used for debt valuation, defined as the maximum of spotPrice and twapPrice",
    "b": "Borrow amount of the asset",
    "c": "Value of collateral supplied for the borrow"
  },
  "english_explanation": "The price used to value a borrowed amount must never be lower than the TWAP price; it must be the higher of the spot and TWAP prices, ensuring that temporary depressions of the spot price cannot reduce the required collateral.",
  "safety_property_type": "Valuation"
}
```