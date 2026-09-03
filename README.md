# Agent Change Confidence Harness V0.1

**Team Preview for a five-person development team using GitHub Copilot**

## Mission

Help a human understand and review an agent-generated code change through bounded authority, source identity, validation evidence, structured logging guidance, and an explicit human decision.

Intended users are developers, reviewers, and a PM or technical lead working together on small, bounded changes.

## Problems Addressed

- An agent can edit more files than intended.
- A test result can be detached from the source that produced it.
- A passing test can be mistaken for complete correctness.
- Logs can accidentally expose identifiers or exception contents.
- Reviewers may lack a compact record of what was requested, changed, tested, and left uncertain.

## What It Does

The harness provides prompts, request and authorization templates, a versioned evidence-packet shape, local shell helpers, and a fictional example. It records source hashes, commands, timestamps, exit codes, expected nonzero search or diff outcomes, validation scope, privacy boundaries, and human review status.

## What It Does Not Prove

It does not prove complete correctness, security, privacy, production readiness, or that an agent followed every instruction. Passing tests prove only the behavior covered by those tests. A packet is evidence for review, not a substitute for review.

## Workflow

1. Write a bounded change request with explicit non-goals and authorized files.
2. Run discovery and establish source identity before implementation.
3. Grant human authorization for the exact scope.
4. Implement only the authorized change and preserve behavior outside it.
5. Capture focused validation with literal commands, working directories, timestamps, logs, and exit codes.
6. Repair evidence only when the PM classifies the evidence as incomplete; preserve superseded evidence first.
7. Compare the protected reference and working copy, excluding only documented generated directories and evidence output.
8. Present the packet, summary, measurements, and uncertainties for human review.
9. Record the human decision and stop at the declared next permitted action.

The draft template is a form, not a valid completed evidence packet: artifact paths are placeholders until real files exist. Run the validator only after populating the packet and evidence directory.

## Team Preview Boundary

V0.1 was validated through one local .NET pilot only. That pilot demonstrates the workflow in one repository and does not establish cross-language, CI, deployment, or organizational reliability.

## Human Approval Boundaries

Human approval must identify the writable workspace, protected reference if one exists, authorized files, allowed validation, prohibited operations, and the next permitted action. The harness does not grant permission. It makes granted scope easier to inspect and enforce.

## Contents

- `INSTALL-GITHUB-COPILOT.md`: local installation and operating guidance
- `prompts/`: discovery, implementation, evidence repair, and PM closeout prompts
- `templates/`: request, authorization, packet, summary, decision, and measurement forms
- `schema/`: JSON Schema for the evidence packet
- `scripts/`: local identity, capture, comparison, and validation helpers
- `examples/`: sanitized fictional evidence
- `LIMITATIONS.md`: known proof boundaries
- `RELEASE-NOTES.md`: V0.1 notes and feedback requested
