{
  "pattern_name": "Minimum Spot/TWAP Selection Leading to Temporal Price Divergence",
  "category": [
    "Temporal State Inconsistency",
    "Oracle Pricing Logic Flaw"
  ],
  "trigger_conditions": [
    "Oracle price function selects the minimum of an instantaneous spot price and a time‑weighted average price",
    "Spot price is sourced from on‑chain AMM pool reserves that can be altered within a single transaction",
    "Borrowing or liquidation logic consumes the oracle's final price without additional sanity checks"
  ],
  "failure_signature": {
    "onchain": [
      "Oracle contract emits a price update where finalPrice equals spotPrice and spotPrice < twapPrice",
      "Borrowing contract records a loan amount using a finalPrice that is lower than the TWAP reference"
    ],
    "offchain": [
      "Monitoring system detects a persistent gap where reported oracle price deviates below the TWAP for the same asset",
      "Historical price feed shows sudden drop in spot price not reflected in TWAP, yet used for debt valuation"
    ]
  },
  "consequence": "State inconsistency in debt valuation, allowing loans to be recorded with a price lower than the stable reference"
}