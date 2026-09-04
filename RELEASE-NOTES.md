# Release Notes

## Version 0.1 - Team Preview

### Included

- Bounded change-request and implementation-authorization templates
- Four prompts for discovery, implementation, evidence repair, and PM closeout
- Source identity, command capture, workspace comparison, and evidence validation scripts
- Structured Change Evidence Packet schema
- Sanitized fictional example

### Lessons Incorporated From Pilot 001

- Hash the protected reference directly when establishing before identity.
- Run a baseline from a disposable copy so the protected reference receives no generated output.
- Record expected nonzero search and diff results separately from execution failures.
- Preserve superseded evidence before an evidence-only correction.
- Keep customer or business identifiers out of structured warning fields while making exception attachment explicit.
- State whether JSON syntax was validated or schema conformance was actually validated.
- Recheck source identity before accepting prior post-change validation as still applicable.
- Treat partial change requests as the normal discovery input: make repository-grounded recommendations, ask for human agreement, and update the request artifact only after clarification is complete.

### Known Limitations

See `LIMITATIONS.md`. In particular, this Team Preview is validated through one local .NET pilot only.

### Feedback Requested

The five-person team should evaluate review time, confidence, evidence completeness, unsupported claims, noise, failure-boundary identification, and follow-up repair precision. Record results in `templates/pilot-measurements.md`.
