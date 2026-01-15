Fail-Open Oracle Validation on Secondary Revert
Summary

The protocol implements a dual-oracle validation mechanism (Primary + Secondary) intended to protect against price manipulation by enforcing deviation checks.

However, the validation logic silently degrades when the Secondary oracle reverts.
If the Secondary source fails, the deviation check is skipped entirely, and the protocol accepts the Primary oracle price without validation.

This creates a fail-open condition where oracle redundancy no longer protects against manipulation.

Threat Model

The protocol relies on oracle prices for critical financial operations, including collateral valuation and borrowing limits.

Attackers are able to:

manipulate on-chain spot prices through flash loans or liquidity distortion,

induce oracle reverts through economic pressure, liquidity removal, or operational failure.

All oracle interactions are permissionless and use standard protocol interfaces.

Assumption

When a Secondary oracle is configured, it provides an independent safety check against manipulation of the Primary oracle.

Deviation checks are assumed to prevent acceptance of a manipulated price.

Failure of an oracle source is assumed not to weaken overall price integrity guarantees.

Invariant

A price must not be accepted from a single oracle source when a secondary validation mechanism exists.

Oracle redundancy must not silently degrade under failure conditions.

Break Point

The deviation check is executed only when both oracle sources successfully return a value.

If the Secondary oracle reverts:

it is marked invalid,

the deviation check is skipped,

the protocol falls back to returning the Primary oracle price without validation.

This breaks the invariant that redundancy protects against single-source manipulation.

Exploit Path

Manipulate the Primary oracle price (e.g., via flash loans on a spot AMM).

Simultaneously induce a revert or failure in the Secondary oracle.

Call the oracle price query function.

Secondary oracle is marked invalid due to revert.

Deviation check is skipped.

Manipulated Primary price is returned and accepted.

All steps use lawful protocol interactions and do not rely on undefined behavior.

Impact

The oracle safety mechanism becomes ineffective under adversarial conditions.

Manipulated prices may be used for:

collateral overvaluation,

under-collateralized borrowing,

extraction of protocol liquidity.

This can result in systemic loss and protocol insolvency.

Fix

Deviation checks must be invariant-preserving.

If a Secondary oracle is configured but fails to respond, the protocol should:

revert the price query, or

explicitly refuse to return an unvalidated price.

Security guarantees must not degrade silently from dual-source validation to single-source trust.

Notes on Reuse

This report represents a canonical instance of a broader failure mode where redundant safety mechanisms fail open under exceptional conditions.

Related abstractions (pattern, invariant, exploit path) can be derived independently from this report.
