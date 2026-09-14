# [Application Name] Change Governance Playbook

- **Status:** DRAFT / APPROVED / SUPERSEDED
- **Application:** [APPLICATION NAME]
- **Governance owner:** [HUMAN NAME OR ROLE]
- **Effective date:** [YYYY-MM-DD]
- **Version:** [VERSION]
- **Supersedes:** [PATH OR NOT_APPLICABLE]
- **External harness directory:** [ABSOLUTE PATH]
- **Project evidence namespace:** [FILESYSTEM-SAFE PROJECT NAME]

## Purpose

This playbook defines how changes to [APPLICATION NAME] move from evidence-backed
understanding to proposal, authorization, implementation, validation, and human
decision. It governs the sequence and decision gates. Individual templates define
the fields recorded at each gate.

Complete the bracketed values before approving this playbook. Preserve an approved
version when revising it; place the prior version under a `superseded/` directory
with a date or version in its filename.

## Application and repository scope

| Repository or component | Role | Absolute workspace path | Protected branch/reference | Evidence namespace | Notes |
|---|---|---|---|---|---|
| [NAME] | [FRONTEND / BACKEND / SERVICE / LIBRARY / OTHER] | [PATH] | [BRANCH, COMMIT, OR HASH MANIFEST] | [NAME] | [NOTES] |

- Environments covered by this playbook: [LOCAL / TEST / OTHER]
- Environments explicitly excluded: [PRODUCTION / OTHER]
- External systems in scope for read-only inspection: [SYSTEMS OR NONE]
- External systems approved for mutation: NONE unless separately authorized
- Regulatory, privacy, or contractual constraints: [DETAILS OR UNKNOWN]
- Repository-specific instruction files: [PATHS OR NONE]

## Governing principles

1. Understand before proposing; propose before authorizing; authorize before
   executing; validate before deciding.
2. Discovery is read-only. Builds, tests, linters, formatters, package restore,
   generators, hosts, and external access require applicable authorization even
   when they are described as checks.
3. A change request is a proposal, not permission. Only a complete, human-granted
   `implementation-authorization.md` permits its named actions.
4. Authorization is exact and temporary: workspace, writable files, commands,
   outputs, external access, expiry condition, and next permitted action must be
   stated explicitly.
5. Authentication, resource access, action-specific authority, transport success,
   and confirmed business completion are independent claims requiring independent
   evidence.
6. Unknown or unavailable evidence is recorded, not inferred. Use `Absent`,
   `Unknown`, `Not applicable` with a reason, and `Not inspected` consistently.
7. Evidence stays under the external harness evidence namespace, never inside the
   application repository unless a separately authorized project rule says otherwise.
8. Prior approval never carries forward to another phase, change, command, commit,
   push, merge, deployment, or external-system mutation.

## Phase applicability

Record the human decision for every phase. `NOT_APPLICABLE` requires a rationale;
it must not be used merely to skip work.

| Phase | Applicability | Required artifact or rationale | Human decision/date |
|---|---|---|---|
| 0 — Repository readiness | REQUIRED / REFRESH_REQUIRED | `repository-readiness.md` | [DECISION] |
| 1 — Existing-logic disposition | REQUIRED / CONDITIONAL / NOT_APPLICABLE | [ARTIFACT PATH OR RATIONALE] | [DECISION] |
| 2 — Consequential-action inventory | REQUIRED / CONDITIONAL / NOT_APPLICABLE | [ARTIFACT PATH OR RATIONALE] | [DECISION] |
| 3 — Cross-repository reconciliation | REQUIRED / CONDITIONAL / NOT_APPLICABLE | [ARTIFACT PATH OR RATIONALE] | [DECISION] |
| 4 — Per-change lifecycle | REQUIRED | One evidence directory per change | [DECISION] |

## Phase 0 — Repository readiness

Produce an evidence-backed, read-only baseline before selecting implementation
work. Follow `prompts/pre-change-repository-readiness.md` and record:

- repository identity, branch/HEAD, and working-tree state;
- stack, architecture, entry points, and established implementation patterns;
- quality-control matrix, including format, lint, static analysis, tests, build,
  scanning, CI, and repository instructions;
- risks, evidence gaps, and bounded hardening recommendations.

- **Required output:** [READINESS REPORT PATH]
- **Exit state:** `READY_FOR_HUMAN_BASELINE_REVIEW`
- **Human selection rule:** recommendations are options, not an automatic work queue.

## Phase 1 — Existing-logic disposition

When applicable, classify meaningful logic against the target operating model:
`Reuse`, `Strengthen`, `Adapt`, `Replace`, `Retire`, or `Missing (build new)`.
Record paths, symbols, dependencies, risk if unchanged, target capability,
confidence, and open questions.

- **Disposition template/path:** [PATH]
- **Scope and coverage rule:** [RULE]
- **Explicit non-goals:** no edits, commands that execute application code,
  installs, or treating current behavior as a requirement merely because it
  exists.
- **Exit state:** [READY_FOR_HUMAN_REVIEW STATUS]

## Phase 2 — Consequential-action inventory

When the application performs state-changing or externally observable actions,
trace each inbound trigger to its consequence boundary. Use stable Action IDs and
two linked tables:

- Table A: trigger, caller, path/symbol, affected resource, classification,
  immediate consequence, and counterpart ID.
- Table B: authentication, resource access, action-specific authority, validation,
  external instruction/destination, immediate result, confirmed outcome,
  idempotency, reversibility, audit evidence, and failure/escalation ownership.

Keep initiation and completion as separate linked actions when separate code paths
establish them. Use `UNRESOLVED` until a counterpart is verified and `NONE` only
after investigation confirms that none exists.

- **Inventory standard/template:** [PATH]
- **Action-ID convention:** [PREFIX AND NUMBERING RULE]
- **Known exclusions:** [PATHS OR NONE]
- **Exit state:** [READY_FOR_HUMAN_REVIEW STATUS]

## Phase 3 — Reconciliation

When multiple repositories, components, or independent reviews describe the same
actions, reconcile them against each other and a fresh source read. Pin material
claims and corrections to repository identity and source hashes. Preserve prior
snapshots under `superseded/` rather than rewriting history.

- **Repositories/components reconciled:** [LIST]
- **Reconciliation artifact:** [PATH]
- **Unmatched or ambiguous relationships:** [DETAILS OR NONE]
- **Exit state:** [READY_FOR_HUMAN_REVIEW STATUS]

## Phase 4 — Per-change lifecycle

Every bounded change follows this sequence:

1. Create a change evidence directory at
   `[HARNESS]/evidence/[PROJECT]/[CHANGE ID]/`.
2. Copy `templates/change-request.md`; this is the canonical request.
3. Select exactly one change profile:
   - `STANDARD` for functional changes or bounded control changes;
   - `LINT_STATIC_ANALYSIS_BASELINE` with
     `templates/lint-static-analysis-baseline.md` for a check-only baseline;
   - `BOUNDED_REFACTOR` with
     `templates/bounded-refactor-change-request.md` for a behavior-preserving
     refactor;
   - `DATABASE_DATA_CONTRACT_REVIEW` with
     `templates/database-data-contract-review.md` for a governed database,
     migration/drift, data-quality, or cross-layer contract review.
4. Complete read-only discovery using `prompts/01-discovery.md`. Record only
   human-accepted or explicitly unresolved values.
5. Obtain a separate, complete `templates/implementation-authorization.md` with
   status `GRANTED` before editing or running commands.
6. Implement only the granted change and capture before/after source identity,
   workspace comparison, and every authorized validation result.
7. Produce `change-evidence-packet.json` and `change-summary.md`; stop at
   `HUMAN_REVIEW_ONLY`.
8. Record the human verdict in `human-decision.md`: `ACCEPTED`,
   `ACCEPTED_WITH_CHANGES`, or `NEEDS_MORE_EVIDENCE`.
9. Record `pilot-measurements.md` when measuring the review experience is useful.

Do not create both focused companions automatically. If linting is only baseline
or post-change validation for a refactor, record it in the bounded-refactor
profile. Use a separate lint-baseline request only when measuring existing lint
health is itself the intended outcome. Automatic lint fixes and broad formatting
remain prohibited unless the canonical request and authorization name their exact
commands and writable files.

## Validation policy

Select checks according to risk and record unavailable validation explicitly.
Do not silently convert an unavailable check into a pass.

| Check | Default requirement | Exact project command/method | Side effects/output paths | Approval needed |
|---|---|---|---|---|
| Source identity before/after | REQUIRED | [METHOD] | [PATHS] | [RULE] |
| Workspace comparison | REQUIRED | [METHOD] | [PATHS] | [RULE] |
| Focused tests | RISK_BASED | [COMMAND] | [SIDE EFFECTS] | [RULE] |
| Full tests | RISK_BASED | [COMMAND] | [SIDE EFFECTS] | [RULE] |
| Lint/static analysis | RISK_BASED | [COMMAND] | [SIDE EFFECTS] | [RULE] |
| Format verification | RISK_BASED | [COMMAND] | [SIDE EFFECTS] | [RULE] |
| Build | RISK_BASED | [COMMAND] | [SIDE EFFECTS] | [RULE] |
| Secret scan | REQUIRED FOR PROPOSED FILES | [METHOD] | Report locations only, never matched values | [RULE] |
| Schema/contract identity | WHEN SHARED CONTRACTS ARE TOUCHED | [METHOD] | [PATHS] | [RULE] |
| Smoke/health check | WHEN RUNTIME CLAIMS ARE MADE | [METHOD] | [PATHS] | [RULE] |
| Privacy/logging review | REQUIRED WHEN LOGGING OR EVIDENCE CHANGES | [METHOD] | [PATHS] | [RULE] |

## Project-specific guardrails

- Protected files or directories: [PATHS OR NONE]
- Prohibited commands or operations: [DETAILS]
- Package and dependency policy: [DETAILS]
- Generated-file policy: [DETAILS]
- Logging and telemetry policy: [DETAILS]
- Personal, confidential, and secret-data exclusions: [DETAILS]
- External-service access policy: [DETAILS]
- Commit, push, PR, merge, and deployment policy: [DETAILS]
- Rollback and recovery expectations: [DETAILS]
- Additional project rules: [DETAILS OR NONE]

## Exceptions, amendments, and deviations

- Environment failures do not widen change scope. Use a narrowly scoped,
  numbered authorization amendment when an environment-only fix is required.
- Scope changes require a revised request and new human decision; they are not
  environment amendments.
- Procedural overreach is recorded in `human-decision.md` even when its outcome
  was harmless.
- Missing governance artifacts remain explicit gaps. Do not reconstruct their
  intended contents from context.
- New evidence may invalidate an earlier assumption. Update downstream requests
  and preserve superseded artifacts rather than defending stale conclusions.

## Artifact map

| Artifact | Purpose | Project location or naming rule |
|---|---|---|
| `repository-readiness.md` | Read-only repository baseline | [PATH/RULE] |
| Disposition map | Existing-logic classification | [PATH/RULE] |
| Consequential-action inventory | Trigger-to-boundary trace | [PATH/RULE] |
| Reconciliation review | Cross-component verification | [PATH/RULE] |
| `change-request.md` | Canonical bounded-change proposal | `[CHANGE ID]/change-request.md` |
| Focused change profile | Lint baseline, bounded-refactor, or database/data-contract review detail | `[CHANGE ID]/[PROFILE].md` |
| `implementation-authorization.md` | Exact human-granted authority | `[CHANGE ID]/implementation-authorization.md` |
| Source identity/workspace comparison | Before/after identity and drift evidence | `[CHANGE ID]/[ARTIFACT].json` |
| Validation logs/results | Literal authorized command evidence | `[CHANGE ID]/validation/` |
| `change-evidence-packet.json` | Machine-checkable change rollup | `[CHANGE ID]/change-evidence-packet.json` |
| `change-summary.md` | Human-readable review narrative | `[CHANGE ID]/change-summary.md` |
| `human-decision.md` | Human verdict and next action | `[CHANGE ID]/human-decision.md` |
| `pilot-measurements.md` | Review-experience measurements | `[CHANGE ID]/pilot-measurements.md` |

## Approval and adoption

- Phase applicability reviewed by: [NAME/ROLE]
- Validation policy reviewed by: [NAME/ROLE]
- Privacy/security constraints reviewed by: [NAME/ROLE OR NOT_APPLICABLE WITH REASON]
- Unresolved governance questions: [QUESTIONS OR NONE]
- Approved status: DRAFT / APPROVED
- Approved by: [HUMAN NAME]
- Approval date: [YYYY-MM-DD]
- Review/expiry boundary: [DATE, RELEASE, OR REPOSITORY CONDITION]
- Next permitted action: [ACTION OR HUMAN_REVIEW_ONLY]

Approval of this playbook establishes a governance process only. It does not
authorize any application edit, validation command, external access, commit,
publication, merge, or deployment.
