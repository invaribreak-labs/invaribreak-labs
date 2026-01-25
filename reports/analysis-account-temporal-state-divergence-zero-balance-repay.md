# System Analysis

## Summary
The credit account contract permits users to specify a prioritized list of liquidation actions. When a repayment action is processed for an asset whose on‑chain balance is zero, the contract returns an error, causing the entire liquidation transaction to abort and leaving the account in its pre‑liquidation state.

## Threat Model
- **threat_source**: Any user of the credit account who can define liquidation preferences.
- **threat_boundary**: The liquidation execution flow that processes user‑defined repayment preferences before other liquidation steps.

## Assumptions
- The contract is deployed on a blockchain that supports CosmWasm execution.
- User‑defined liquidation preferences are processed sequentially during a liquidation call.
- The query_balance function returns the current token amount held by the credit account for a given denomination.
- An error returned from the liquidation loop propagates via the ? operator and aborts the transaction.
- Zero token balances are possible when a user withdraws all of a particular asset before liquidation.

## System Model
### Components
- Credit account smart contract that maintains user balances and liquidation preferences.
- Liquidation engine that iterates over the preference queue.
- Blockchain query module used to retrieve token balances.
- User‑provided liquidation preference messages (e.g., Repay actions).

