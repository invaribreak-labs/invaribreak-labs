# System Analysis

## Summary
The BaseOracle contract derives an asset's USD price by selecting the minimum of a spot price obtained from an AMM pool's current reserves and a time‑weighted average price (TWAP) supplied by an external aggregator. The spot price is read via a getReserves() call, while the TWAP reflects a longer observation window. The resulting price is used by the borrowing contract for debt valuation.

## Threat Model
- **threat_source**: External transaction that can modify the AMM pool's reserves within the same block execution.
- **threat_boundary**: Oracle price computation that combines the instantaneous spot price with the TWAP value.

## Assumptions
- The AMM pool implements a standard getReserves() interface returning current reserves.
- The TWAP aggregator provides a price that is not affected by a single transaction.
- BaseOracle reads both the spot price and the TWAP during a single transaction before returning the final price.
- The borrowing contract relies on the price returned by BaseOracle to assess the value of debt.

## System Model
### Components
- BaseOracle contract
- Automated Market Maker (AMM) pool
- TWAP aggregator contract
- Borrowing contract
- EVM execution environment

## Break Point
> **Intended State**: Debt valuation price = max(maxSpotUsdValue, minTwapUsdValue)
>
> **Actual State**: Debt valuation price = min(maxSpotUsdValue, minTwapUsdValue)
>
> **Divergence**: maxSpotUsdValue < minTwapUsdValue

