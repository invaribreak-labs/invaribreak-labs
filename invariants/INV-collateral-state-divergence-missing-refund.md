{
  "invariant_id": "INV-001-collateral-consistency",
  "formal_statement": "∀ t ∈ Traders : Portfolio.locked[t] = Σ_{o ∈ OpenOrders(t)} (o.price × o.quantity)",
  "variables": {
    "t": "Identifier of a trader (maker) in the system",
    "Portfolio.locked[t]": "Total amount of quote token currently locked for trader t in the Portfolio contract",
    "OpenOrders(t)": "Set of limit orders belonging to trader t that are not yet filled or cancelled",
    "o.price": "Limit price specified in order o (quote token per base token)",
    "o.quantity": "Quantity of base token specified in order o"
  },
  "english_explanation": "For every trader, the locked collateral recorded in the Portfolio contract must always equal the sum of (limit price × quantity) of that trader's active limit orders. After an order is filled, any surplus portion of the originally locked amount must be released so that no locked balance remains unrelated to open orders.",
  "safety_property_type": "Conservation"
}