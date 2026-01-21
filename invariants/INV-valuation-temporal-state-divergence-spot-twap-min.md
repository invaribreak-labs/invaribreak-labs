{
  "invariant_id": "INV-BASEORACLE-VAL-001",
  "formal_statement": "price_debt = max(spotPrice, twapPrice)",
  "variables": {
    "price_debt": "USD price used by the borrowing contract to value the debt",
    "spotPrice": "Instantaneous spot price obtained from the AMM pool via getReserves()",
    "twapPrice": "Time‑weighted average price supplied by the TWAP aggregator"
  },
  "english_explanation": "The debt valuation must always use the greater of the instantaneous spot price and the TWAP, ensuring that a transient low spot price cannot reduce the reported value of the borrowed asset.",
  "safety_property_type": "Valuation"
}