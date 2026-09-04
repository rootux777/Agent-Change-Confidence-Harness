# Agent Change Confidence Harness V0.1

**Team Preview for a five-person development team using coding agents**

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

The harness provides prompts, request and authorization templates, a versioned evidence-packet shape, local shell helpers, and a fictional example. It records source hashes, commands, timestamps, exit codes, expected nonzero search or diff outcomes, validation scope, privacy boundaries, approved code-comment and application-logging scope, and human review status.

## What It Does Not Prove

It does not prove complete correctness, security, privacy, production readiness, or that an agent followed every instruction. Passing tests prove only the behavior covered by those tests. A packet is evidence for review, not a substitute for review.

## Workflow

1. Copy the change-request template and fill in whatever is already known.
2. Submit the partial request to the read-only discovery prompt.
3. The agent reads the request, inspects the relevant local code and tests, identifies missing values, and makes concrete recommendations where repository evidence supports them.
4. The human accepts, rejects, or revises each recommendation. The agent repeats clarification until every required request field is complete.
5. After human agreement, the agent may update the named request artifact with the agreed values. This does not grant implementation authority.
6. Complete implementation authorization separately and grant only the exact scope.
7. Establish source identity before implementation.
8. Implement only the authorized change and preserve behavior outside it.
9. Capture focused validation with literal commands, working directories, timestamps, logs, and exit codes.
10. Repair evidence only when the PM classifies the evidence as incomplete; preserve superseded evidence first.
11. Compare the protected reference and working copy, excluding only documented generated directories and evidence output.
12. Present the packet, summary, measurements, and uncertainties for human review.
13. Record the human decision and stop at the declared next permitted action.

The draft template is a form, not a valid completed evidence packet: artifact paths are placeholders until real files exist. Run the validator only after populating the packet and evidence directory.

The change request is intentionally different from the implementation authorization. A partial request is an input to an interactive discovery conversation; it is not permission to infer scope or edit code. Only explicit human agreement completes the request, and only the separate authorization artifact permits implementation.

## Team Preview Boundary

V0.1 was validated through one local .NET pilot only. That pilot demonstrates the workflow in one repository and does not establish cross-language, CI, deployment, or organizational reliability.

## Human Approval Boundaries

Human approval must identify the writable workspace, protected reference if one exists, authorized files, allowed validation, prohibited operations, and the next permitted action. The harness does not grant permission. It makes granted scope easier to inspect and enforce.

## Template and Prompt Map

Copy templates into the change's evidence directory under the external harness checkout; do not edit the reusable files under `templates/`, and do not create harness artifacts in the application repository. A human owns the template artifacts. Prompts tell an agent how to use those artifacts without granting new authority.

| Human artifact | Associated prompt | Purpose |
| --- | --- | --- |
| A [repository-readiness report](docs/pre-change-repository-readiness.md), such as `<harness-path>/evidence/<project-name>/readiness/<assessment-id>/repository-readiness.md` | [Pre-Change Repository Readiness Prompt](prompts/pre-change-repository-readiness.md) | Establishes a read-only architecture, history, quality-control, and candidate-pattern baseline before a change request. It never grants implementation or hardening authority. |
| A copied [change request](templates/change-request.md), such as `<harness-path>/evidence/<project-name>/<change-id>/change-request.md` | [Discovery Prompt](prompts/01-discovery.md) | Gives a read-only agent the human's intent and boundaries. The agent identifies the owning code path, candidate files, conventions, validation, and open human decisions. |
| A copied [implementation authorization](templates/implementation-authorization.md) | [Implementation Prompt](prompts/02-implementation.md) | Gives an edit-permitted agent the exact writable workspace, files, commands, prohibitions, and next action approved by a human. |
| A completed [evidence packet](templates/change-evidence-packet.json) plus its evidence artifacts | [Evidence Repair Prompt](prompts/03-evidence-repair.md) | Allows evidence-only correction after a human classifies the evidence as incomplete; it must not change implementation source. |
| A completed evidence packet, summary, and validation logs | [PM Closeout Prompt](prompts/04-pm-closeout.md) and [human decision](templates/human-decision.md) | Supports independent human review and records the next permitted decision. |

Use the actual artifact path in the chat prompt. `<harness-path>/evidence/<project-name>/<change-id>/change-request.md` is the canonical request-file pattern; `discovery.md` is generated after discovery, not copied from `prompts/`.

## Contents

- `INSTALL-GITHUB-COPILOT.md`: local installation and operating guidance
- `prompts/`: setup, readiness assessment, discovery, implementation, evidence repair, and PM closeout prompts
- `docs/`: readiness-assessment guidance and report template
- `templates/`: request, authorization, packet, summary, decision, and measurement forms
- `schema/`: JSON Schema for the evidence packet
- `scripts/`: local identity, capture, comparison, and validation helpers
- `examples/`: sanitized fictional evidence
- `LIMITATIONS.md`: known proof boundaries
- `RELEASE-NOTES.md`: V0.1 notes and feedback requested
