# Invariant: account-temporal-state-divergence-zero-balance-repay

## Formal Statement
`∀ a ∈ Accounts, ∀ d ∈ Denoms, ∀ i ∈ PrefIndices. (Pref[i] = Repay(d) ∧ Balance(a, d) = 0) ⇒ (¬Abort(Liquidation(a)) ∧ NextPrefProcessed(a, i+1))`

## English Explanation
When the liquidation engine processes a Repay preference for a token that the account holds zero of, the transaction must not abort. Instead, the Repay step is skipped and the engine continues with the next preference, guaranteeing that liquidation can always make progress.

## Variables
- **a**: Credit account address
- **d**: Denomination of the token to be repaid
- **i**: Index of the current liquidation preference in the user‑defined queue
- **Pref[i]**: The i‑th liquidation preference message (e.g., Repay(d))
- **Balance(a, d)**: Current on‑chain token amount of denomination d held by account a
- **Abort(Liquidation(a))**: The liquidation transaction for account a terminates without applying any state changes
- **NextPrefProcessed(a, i+1)**: The liquidation engine proceeds to evaluate the preference at index i+1 for account a

## Safety Property Type
Unknown

## Counterexample State
> undefined


## References
- [Day 1 Report](../reports/analysis-account-temporal-state-divergence-zero-balance-repay.md)
