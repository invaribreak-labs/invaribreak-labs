# Analysis Structure

All protocol analyses in this repository follow a consistent structure to make assumptions, failure conditions, and impacts explicit.

This structure reflects how the analyses are conducted and is provided for transparency and reuse.

---

## Threat Model

Defines the environment and actors considered in the analysis.
Includes constraints such as permissions, capital requirements, and protocol rules.

## Assumption

States the assumptions the protocol relies on for correct operation.
Assumptions may be economic, technical, or behavioral.

## Invariant

Describes the condition that must continuously hold for the system to remain stable.
An invariant is stronger than an assumption and is expected to remain true across states.

## Break Point

Identifies the condition under which the invariant no longer holds.
This is the moment where system behavior changes qualitatively.

## Exploit Path

Outlines the sequence of actions that becomes possible once the invariant is broken.
The path may involve only lawful protocol interactions.

## Impact

Explains the resulting effect on the system or its participants.
Impacts may be economic, structural, or governance-related.

## Fix

Discusses potential mitigations or design changes.
Fixes are not prescriptions, but considerations tied to the broken invariant.

---

This structure is intended to support clear reasoning about protocol failure modes and to enable consistent discussion across analyses.
