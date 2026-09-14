# Initial Grouped Action

Use this prompt to begin or resume a project review using the Agent Change Confidence Harness. Replace bracketed values; leave unavailable items as `UNKNOWN`.

For a new application, use this after completing the setup described in
`vscode-instructions.md`. In **Application governance playbook**, provide the path
to the approved project-specific copy when one exists. Otherwise point to
`templates/application-change-governance-playbook.md`; the agent may use its
sequence and default guardrails but must treat its unfilled project decisions as
unresolved. Describe the currently intended stage rather than asking the agent to
run the entire lifecycle automatically.

```markdown
You are helping assess and improve an application using the Agent Change
Confidence Harness.

## Project context

- Application name: [APPLICATION NAME]
- Human reviewer and decision-maker: [NAME]
- Application workspace: [ABSOLUTE PATH]
- External harness directory: [ABSOLUTE PATH]
- Project evidence namespace: [FILESYSTEM-SAFE PROJECT NAME]
- Application governance playbook: [PROJECT-SPECIFIC COPY PATH OR HARNESS DIRECTORY/templates/application-change-governance-playbook.md]
- Governance status: [APPROVED / DRAFT / UNKNOWN]
- Intended target branch: [BRANCH OR UNKNOWN]

## Previous work, if any

- Latest readiness report: [PATH OR NONE]
- Latest application-entry-point report: [PATH OR NONE]
- Latest consequential-action inventory and reconciliation: [PATH OR NONE]
- Accepted Action, Bridge, or Evidence IDs relevant to current intent: [IDS OR NONE]
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
- Read and follow the application governance playbook named in Project context.
  If it is the reusable unfilled template rather than an approved project-specific
  copy, use its sequence and default guardrails but treat every bracketed value
  and project-specific policy as unresolved; it grants no authority.
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

Read the application governance playbook named in Project context. Confirm its
status, repository/component scope, phase-applicability decisions, evidence
locations, and project-specific guardrails. Do not infer approval from the file's
existence or from a reusable template's default text.

Verify the application and harness locations using permitted read-only
inspection. Report actual setup gaps rather than an unsupported READY status.
Do not modify repository instructions without authorization.

For an existing project, inspect relevant prior reports and decisions.
Compare their repository identity and HEAD with the current checkout.
Identify stale evidence and unresolved decisions.

Return a concise checkpoint containing:
- Repository, branch, HEAD, and working-tree state
- Governance playbook status and applicable current phase
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

### 3. Decide whether consequential-action inventory and reconciliation apply

Use the human-reviewed readiness and entry-point evidence with the phase rules in
the application governance playbook. Recommend the inventory when the application:

- Changes material state or sends externally consequential instructions
- Affects customers, money, entitlements, security, or regulated records
- Spans components that must agree about actor, resource, authority, instruction,
  transport result, or confirmed business outcome
- Implements initiation, confirmation, cancellation, or compensation in separate
  code paths

Do not require the inventory mechanically for an isolated task with no relevant
consequential flow. If it is not applicable, obtain and record the human's reason;
do not invent or silently assume that decision.

When it applies, ask the human for an Inventory ID. With explicit initialization
permission, create only:
[HARNESS DIRECTORY]/evidence/[PROJECT NAME]/discovery/[INVENTORY ID]/

Copy:
[HARNESS DIRECTORY]/templates/consequential-action-inventory.md
to:
[HARNESS DIRECTORY]/evidence/[PROJECT NAME]/discovery/[INVENTORY ID]/consequential-action-inventory.md

Follow the instructions in the project-specific copy. Remain read-only in every
application repository. Populate the component registry, Evidence manifest,
Action Tables A and B, Bridge table, reconciliation summary, coverage statement,
and consistency checks. Inspect both sides before marking a Bridge `VERIFIED`.

SHA-256 values are content fingerprints for exact cited source files, not permanent
file IDs or proof of malicious tampering. A hash mismatch makes dependent evidence
stale until the changed source is reread and reconciled. Hash an entire declared
scope only when the human explicitly selects and bounds full-scope drift detection.

Stop at `HUMAN_REVIEW_ONLY`. The human must accept, revise, or request more evidence
before an inventory finding is used to select a bounded change. The inventory is
baseline evidence; it is not a change request or implementation authorization.

### 4. Select and initialize a bounded change

Help translate my intended outcome or one accepted baseline finding into one
narrow, observable behavior. Cite the applicable readiness recommendation and
accepted Action, Bridge, or Evidence IDs. Confirm their repository identities and
hashes remain current before relying on them.

Ask for a new change ID when needed. With explicit initialization permission,
create only:
[HARNESS DIRECTORY]/evidence/[PROJECT NAME]/[CHANGE ID]/

Copy templates/change-request.md into that directory.
The copied `change-request.md` is the canonical request for every change.
Select exactly one change profile and, when applicable, copy one companion:

- For a check-only run of existing linting or static analysis, select
  `LINT_STATIC_ANALYSIS_BASELINE` and copy
  `templates/lint-static-analysis-baseline.md`.
- For a behavior-preserving refactor, select `BOUNDED_REFACTOR` and copy
  `templates/bounded-refactor-change-request.md`.
- For a database, migration/drift, data-quality, or cross-layer contract review,
  select `DATABASE_DATA_CONTRACT_REVIEW` and copy
  `templates/database-data-contract-review.md`.
- For a migration proposed after an accepted database/data-contract review, use a
  new Change ID, select `DATABASE_MIGRATION`, and copy
  `templates/database-migration-change-request.md`. Review findings and prior
  authorization do not authorize the migration.
- For other bounded work, select `STANDARD`; no companion is required.

Record the companion's change-specific path in the canonical request. A
companion adds discovery detail but is not a second request or an authorization.
Do not create both companions automatically. When linting is only baseline or
post-change validation for a refactor, record it in the bounded-refactor profile.
Use a separate lint-baseline request when measuring the existing lint state is
itself the intended outcome.

### 5. Conduct change-specific discovery

Follow:
[HARNESS DIRECTORY]/prompts/01-discovery.md

Inspect relevant source and nearby tests read-only. Recommend:
- Owning path and exact candidate files
- Required and preserved behavior
- A falsifiable hypothesis and a cheap disconfirming check
- Validation commands and their side effects
- Privacy exclusions, non-goals, and rollback

For a lint/static-analysis baseline, keep execution check-only: no automatic
fixes, broad formatting, source/configuration edits, dependency installation, or
unrelated cleanup. If findings warrant edits, propose a separate bounded change.

For a bounded refactor, identify observable behavior invariants and supporting
regression checks. Linting may be proposed as pre-change evidence,
post-change validation, or both, but lint-driven edits and broad formatting stay
prohibited unless the canonical request and authorization explicitly include
their commands and exact writable files.

Ask me to accept, reject, or revise unresolved choices.
Record only agreed values. A completed request is not implementation approval.

### 6. Implement only after separate authorization

The human completes and approves a change-specific copy of
templates/implementation-authorization.md.

Verify its workspace, files, commands, evidence destination, boundaries,
and next permitted action before following prompts/02-implementation.md.

Capture required source identity and validation evidence, preserve
superseded artifacts, validate the evidence packet, and stop at
HUMAN_REVIEW_ONLY.

### 7. Close out and continue

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
