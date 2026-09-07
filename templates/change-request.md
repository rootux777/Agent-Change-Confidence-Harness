# Change Request

- Change ID:
- Requested by:
- Intent:
- Owning code path:
- Authorized workspace:
- Protected reference:
- Authorized files:
- Required behavior:
- Behavior that must remain unchanged:
- Comment expectations (only for non-obvious intent):
- Approved application logging or telemetry events:
- Approved structured fields:
- Prohibited data in logs/evidence:
- Validation command(s):
- Build command, if authorized:
- Non-goals:
- Rollback:
- Requested human decision:

## Readiness Context and First-Change Scope

For a readiness-driven request, complete these fields from the saved report and human review. Otherwise mark this section `NOT_APPLICABLE` with a reason. This is part of the canonical change request, not a separate request or authorization.

- Readiness report path (external Markdown artifact):
- Assessment ID and recorded repository identity / HEAD:
- Recorded working-tree state and relevant local changes:
- Human baseline review (reviewer, date, accepted context or unresolved concerns):
- Selected recommendation ID and report section:
- Evidence supporting this selection:
- Current repository identity / HEAD and working-tree comparison (discovery):
- First-change type: CHECK_ONLY_BASELINE / ADD_OR_ADJUST_CONTROL / BOUNDED_REFACTOR / FUNCTIONAL_CHANGE
- Intended measurable outcome and acceptance criteria:
- Existing findings policy (record baseline, permitted new findings, deferred findings):
- Exact check commands and working directories (proposed until authorized):
- Prerequisites and command side effects (caches, generated output, dependency or network needs):
- External validation evidence destination:
- Refactor behavior invariants and supporting tests, or NOT_APPLICABLE:
- Deferred recommendations and explicit non-goals:

Prefer one existing lint or static-analysis check in check-only mode when the report supports it. Do not assume commands are free of writes; identify caches and generated files. Adding tooling or refactoring is a separate scope decision. Do not bundle automatic fixes, broad formatting, or unrelated cleanup into a baseline check. For check-only work, explicitly state that no source edits are requested and name any permitted output paths. Leave unsupported values unresolved for discovery.

## Discovery Completion

This request may be submitted partially completed. During read-only discovery, the agent identifies missing values, inspects the local codebase, makes evidence-based recommendations, and asks the human to accept or revise them. The agent may update this request file only after the human agrees to the complete set of values. Completing this section does not grant implementation authorization.

- Human clarification decisions recorded:
- Agent recommendations accepted or revised:
- Request complete: YES / NO
