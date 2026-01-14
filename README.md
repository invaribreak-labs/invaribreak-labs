# Invaribreak Labs

Invariant-based protocol risk analyses focused on attack surfaces, incentive failures, and lawful exploit paths.

## Overview

Invaribreak Labs publishes structured analyses of protocol failure modes under explicit assumptions and invariants.

The work here is not centered on individual vulnerabilities alone, but on **how systems fail once their underlying assumptions no longer hold**, including:

* protocol attack surfaces created by design constraints,
* incentive misalignments that lead to rational but harmful behavior,
* exploit paths that arise from lawful use of protocol rules.

All analyses are presented in a consistent structure to support review, discussion, and reuse.

## Scope

This repository contains:

* protocol-level risk analyses,
* recurring failure patterns observed across systems,
* invariant assumptions and their breakpoints,
* documented exploit paths that do not rely on undefined behavior.

## What this repository is not

* This is **not** a formal security audit.
* This is **not** a guarantee of protocol safety.
* This is **not** a vulnerability disclosure service.

The materials are intended for research, design review, and risk reasoning.

## Structure

* `reports/` — protocol-specific analyses
* `patterns/` — recurring structural failure patterns
* `invariants/` — invariant assumptions and failure conditions
* `exploit-paths/` — lawful exploit and extraction paths
* `analysis-structure.md` — the analysis structure used across reports

## Usage

Readers are encouraged to:

* examine assumptions and invariants critically,
* reuse the analysis structure for their own reviews,
* challenge or extend the identified breakpoints.

Contributions are welcome in the form of structured analysis or discussion.

---
