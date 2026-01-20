{
  "pattern_name": "Collateral State Divergence without Surplus Refund",
  "category": [
    "Temporal State Violation",
    "Resource Accounting Inconsistency"
  ],
  "trigger_conditions": [
    "A limit buy order is matched at an execution price lower than the order's limit price.",
    "The matching routine calls the collateral deduction routine using the execution price.",
    "No subsequent routine adjusts the remaining locked collateral for the maker after execution."
  ],
  "failure_signature": {
    "onchain": [
      "Portfolio.locked[maker] > Σ_{openOrders} (order.price × order.quantity) after the order is marked filled",
      "Absence of a balance‑adjustment event following the addExecution call"
    ],
    "offchain": [
      "Monitoring system reports a mismatch between a trader's locked collateral and the total of their active limit orders",
      "Audit logs show order completion without a corresponding surplus‑release entry"
    ]
  },
  "consequence": "State inconsistency where locked collateral remains allocated despite no corresponding active order, leading to permanently retained funds."
}