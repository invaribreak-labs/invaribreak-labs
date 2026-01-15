# Analysis Structure

Invaribreak Labs protocol analyses follow a **canonical invariant-based structure**
designed to make assumptions, failure conditions, and system impacts explicit.

This structure reflects how analyses are conducted and is provided
to enable transparency, comparison, reuse, and citation across protocols.

---

## Threat Model

Defines the environment and actors considered in the analysis.

Includes constraints such as permissions, capital requirements,
economic roles, and protocol rules.

---

## Assumption

States the assumptions the protocol relies on for correct operation.

Assumptions may be economic, technical, temporal, or behavioral,
and are often implicit in protocol design.

---

## Invariant

Describes the condition that must continuously hold
for the system to remain stable and coherent.

An invariant is stronger than an assumption and is expected
to remain true across state transitions.

---

## Break Point

Identifies the condition under which the invariant no longer holds.

This marks the moment where system behavior changes qualitatively
and previously safe actions become unsafe.

---

## Exploit Path

Outlines the sequence of actions that becomes possible
once the invariant is broken.

Exploit paths may involve **only lawful, rules-compliant protocol interactions**,
without privileged access, undefined behavior, or rule violations.

---

## Impact

Explains the resulting effect on the system or its participants.

Impacts may be economic, structural, governance-related,
or affect incentive alignment.

---

## Fix

Discusses potential mitigations or design considerations
tied directly to the broken invariant.

Fixes are not prescriptions,
but explorations of how the invariant could be restored or enforced.

---

This structure is intended to support precise reasoning about protocol failure modes
and to enable consistent discussion and reuse across analyses and systems.
