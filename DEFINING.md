# Defining the Work

## What this framework is

This work is an invariant-based approach to protocol risk analysis.

Each analysis focuses on:

* explicit **assumptions** made by the protocol,
* **invariants** that are expected to hold under normal operation,
* **break points** where those invariants fail,
* resulting **exploit paths** that emerge without violating protocol rules.

The goal is to make failure conditions legible and discussable, rather than to enumerate isolated bugs.

## What this framework is not

* It is not a checklist-driven security review.
* It does not aim to cover all possible vulnerabilities.
* It does not replace formal verification or audits.
* It does not evaluate market success, team competence, or intent.

The framework is concerned only with system behavior under defined constraints.

## Why invariants

Protocols often rely on implicit invariants:

* economic assumptions,
* timing assumptions,
* trust and coordination assumptions.

When these invariants break, failures tend to be systemic rather than local.
This work surfaces those invariants explicitly and examines their failure modes.

## Intended use

This framework is intended for:

* protocol designers reviewing system assumptions,
* security researchers analyzing structural risk,
* reviewers discussing failure conditions with shared terminology.

The structure is reusable and protocol-agnostic.

---
