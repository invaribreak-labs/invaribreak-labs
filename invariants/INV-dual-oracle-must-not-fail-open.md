# Invariant: Dual Oracle Must Not Fail Open

## Invariant

If a protocol configures multiple oracle sources to enforce price integrity, the failure of any configured safety source MUST NOT result in unchecked acceptance of another source.

## Rationale

Dual-oracle designs are commonly used to mitigate manipulation risk by:

- cross-validating prices across independent sources,
- enforcing deviation bounds,
- degrading availability only when safety guarantees remain intact.

Allowing the system to proceed with a single source **after explicitly configuring redundancy** breaks the core safety assumption of the design.

## Violation Condition

- A secondary oracle is configured as a safety or validation source.
- The secondary oracle fails or reverts.
- The protocol suppresses the failure and proceeds using the primary oracle **without enforcing deviation or consistency checks**.

## Systemic Consequence

- Redundancy collapses into a single-point oracle.
- Manipulation resistance assumptions no longer hold.
- Oracle integrity becomes contingent on availability rather than correctness.

This invariant violation enables lawful price manipulation paths that bypass the intended safety architecture.

## Canonical Reference

This invariant abstraction derives from the following canonical report:

- Canonical analysis:  
  `../reports/dual-oracle-fail-open-on-secondary-revert.md`
