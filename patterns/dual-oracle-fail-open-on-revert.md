Description

A protocol employs multiple oracle sources to improve price reliability.
However, safety checks between sources are conditionally executed only when all sources respond successfully.

If one oracle reverts or fails, the protocol suppresses the error and proceeds by accepting the remaining source without enforcing cross-validation.

This results in a fail-open security posture.

Common Preconditions

- Primary and secondary oracle sources are configured.
- Deviation or consistency checks require both sources to be valid.
- Oracle calls are wrapped in try/catch or equivalent error suppression.
- Failure of a safety source does not trigger a revert.
  
Failure Mode

- Oracle failure is treated as an availability issue rather than a security violation.
- Safety logic is skipped instead of enforced.
- The system silently degrades into a weaker security configuration.
  
Observable Symptoms

- Price feeds remain available despite partial oracle failure.
- Deviation checks are skipped under failure conditions.
- Manipulated prices are accepted without explicit error signaling.
  
Why This Pattern Is Dangerous

Fail-open oracle designs preserve liveness at the cost of correctness.
This inverts the intended security tradeoff and enables rational attackers to combine:

- price manipulation on the primary source, and
- denial-of-service on the secondary source.
  
Canonical Reference

This pattern is generalized from the following canonical report:

- Canonical analysis:
https://github.com/invaribreak-labs/invaribreak-labs/blob/a49e6cdd99d988ecaa85dd32435b6bc7edde0f4e/reports/dual-oracle-fail-open-on-secondary-revert.md
