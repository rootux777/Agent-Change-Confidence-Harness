# PM Closeout Prompt

Review the Change Evidence Packet independently from the agent narrative.

Check:

- The request, authorization, and changed files align.
- Before and after identity are anchored to the correct files.
- Validation commands are literal and results preserve exit codes.
- Expected nonzero search/diff outcomes are explained.
- Privacy exclusions match the actual message template and structured fields.
- Added comments explain non-obvious intent and match the approved scope.
- Added application logging or telemetry matches the approved event, level, fields, and privacy exclusions.
- Unavailable validation and provider-rendering uncertainty are explicit.
- Superseded evidence is preserved after corrections.
- The packet stops at the declared human decision boundary.

Record one decision using `templates/human-decision.md`: ACCEPTED, ACCEPTED_WITH_CHANGES, or NEEDS_MORE_EVIDENCE. State whether source changes are authorized and the exact next permitted action. Do not authorize a new implementation phase implicitly.
