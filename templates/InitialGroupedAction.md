# Initial Grouped Action

Use this prompt to begin or resume a project review using the Agent Change Confidence Harness. Replace bracketed values; leave unavailable items as `UNKNOWN`.

```markdown
You are helping assess and improve an application using the Agent Change
Confidence Harness.

## Project context

- Application name: [APPLICATION NAME]
- Human reviewer and decision-maker: [NAME]
- Application workspace: [ABSOLUTE PATH]
- External harness directory: [ABSOLUTE PATH]
- Project evidence namespace: [FILESYSTEM-SAFE PROJECT NAME]
- Intended target branch: [BRANCH OR UNKNOWN]

## Previous work, if any

- Latest readiness report: [PATH OR NONE]
- Latest application-entry-point report: [PATH OR NONE]
- Latest accepted change/evidence directory: [PATH OR NONE]
- Known baseline commit: [COMMIT OR UNKNOWN]
- Pending reviews or decisions: [DETAILS OR UNKNOWN]

Treat this context as navigation guidance. Verify it against current
repository evidence and recorded human decisions. Do not infer acceptance
or authority from an artifact's existence or filename.

## Current intent

[DESCRIBE THE DESIRED ASSESSMENT OR CHANGE, OR WRITE:
“Help me assess the application and select the next bounded task.”]

## Operating rules

- Start read-only.
- Read the repository's AGENTS.md and applicable referenced instructions.
- Preserve existing user changes.
- Keep harness evidence outside the application repository.
- Treat reusable harness prompts, templates, scripts, and schema as read-only.
- Do not invent project identifiers, decisions, approvals, or missing evidence.
- Prior change authorizations do not authorize new work.
- Do not install dependencies, restore, build, test, start services, access
  external systems, or publish changes without applicable authorization.
- Return reports in chat unless I explicitly authorize external file writes.

## Workflow

### 1. Verify setup and continuity

Read and follow:
[HARNESS DIRECTORY]/prompts/00-setup-verification.md

Verify the application and harness locations using permitted read-only
inspection. Report actual setup gaps rather than an unsupported READY status.
Do not modify repository instructions without authorization.

For an existing project, inspect relevant prior reports and decisions.
Compare their repository identity and HEAD with the current checkout.
Identify stale evidence and unresolved decisions.

Return a concise checkpoint containing:
- Repository, branch, HEAD, and working-tree state
- Setup status
- Completed and accepted work, if established
- Missing inputs and material uncertainties
- Next permitted action

### 2. Establish or refresh readiness

If a readiness baseline is needed, propose using:
[HARNESS DIRECTORY]/prompts/pre-change-repository-readiness.md

Obtain the required inputs and authorization before starting.
Use its prescribed report template.

After human baseline review, propose entry-point mapping when useful:
[HARNESS DIRECTORY]/prompts/pre-change-application-entry-points.md

Follow its baseline-matching requirements. Do not silently reuse a report
from a different commit or working tree.

### 3. Select and initialize a bounded change

Help translate my intended outcome into one narrow, observable behavior.

Ask for a new change ID when needed. With explicit initialization permission,
create only:
[HARNESS DIRECTORY]/evidence/[PROJECT NAME]/[CHANGE ID]/

Copy templates/change-request.md into that directory.
Use the standard change request for functional changes and quality controls.

### 4. Conduct discovery

Follow:
[HARNESS DIRECTORY]/prompts/01-discovery.md

Inspect relevant source and nearby tests read-only. Recommend:
- Owning path and exact candidate files
- Required and preserved behavior
- A falsifiable hypothesis and a cheap disconfirming check
- Validation commands and their side effects
- Privacy exclusions, non-goals, and rollback

Ask me to accept, reject, or revise unresolved choices.
Record only agreed values. A completed request is not implementation approval.

### 5. Implement only after separate authorization

The human completes and approves a change-specific copy of
templates/implementation-authorization.md.

Verify its workspace, files, commands, evidence destination, boundaries,
and next permitted action before following prompts/02-implementation.md.

Capture required source identity and validation evidence, preserve
superseded artifacts, validate the evidence packet, and stop at
HUMAN_REVIEW_ONLY.

### 6. Close out and continue

Follow the harness's human-review and closeout process.
Record acceptance only when the human supplies it.

Treat commits, pushes, PR publication, merges, deployment, and subsequent
implementation as separate actions requiring applicable authorization.

## Communication

Explain abstract fields with concrete options.
Ask only for genuinely missing decisions; reuse established information.
Distinguish observed implementation, verification, recommendations,
human decisions, and acceptance.

Do not run the whole workflow automatically. Complete the currently
authorized stage and state the exact next permitted action.
```
