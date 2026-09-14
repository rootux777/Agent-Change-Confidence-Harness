# Bounded Refactor Change-Request Profile

Use this companion with a change-specific copy of `change-request.md` when the
selected change profile is `BOUNDED_REFACTOR`. The canonical change request
remains the source of truth. This profile makes the behavior-preservation claim,
edit boundary, and regression evidence explicit before implementation.

Completing this profile does not authorize edits, builds, tests, linters,
formatters, package restore, network access, commits, or publication.

## Request linkage

- Change ID:
- Canonical change-request path:
- Motivating readiness/discovery finding:
- Repository root:
- Protected reference and working-tree state:
- Human reviewer:

## Refactor objective and boundary

- Structural problem being addressed:
- Intended maintainability or quality outcome:
- Owning code path and symbols:
- Candidate writable files:
- Reference-only files:
- Affected callers, consumers, or boundaries:
- Explicit non-goals:

## Behavior-preservation contract

List each observable invariant separately. Avoid statements such as "behavior
unchanged" without naming how a reviewer can observe it.

| Invariant ID | Behavior that must remain unchanged | Current evidence | Regression test or check | Material uncertainty |
|---|---|---|---|---|
| INV-001 |  |  |  |  |

- Public API/contract changes permitted: NO / explicitly describe
- Data/schema changes permitted: NO / explicitly describe
- Dependency changes permitted: NO / explicitly describe
- Logging/telemetry changes permitted: NO / explicitly describe approved events and fields
- Performance or concurrency characteristics that must be preserved:
- Error, retry, and rollback behavior that must be preserved:

## Proposed refactoring mechanism

- Proposed structural change:
- Why it is the smallest change that meets the objective:
- Existing repository pattern to follow:
- Identifier/conflict checks required:
- Planned edit sequence and intermediate-state risks:
- Rollback procedure tied to the protected reference:

## Linting and static analysis

Linting may be authorized as pre-change baseline evidence, post-change
validation, or both. Its presence here does not authorize lint-driven edits.

- Role: PRE_CHANGE_BASELINE / POST_CHANGE_VALIDATION / BOTH / NOT_APPLICABLE
- Exact check-only commands and working directories:
- Expected exit semantics and existing-findings policy:
- Command prerequisites and side effects:
- Permitted evidence/output paths:
- Automatic fix mode: PROHIBITED / SEPARATELY_REQUESTED
- Broad formatting: PROHIBITED / SEPARATELY_REQUESTED
- If fixes or formatting are separately requested, exact files and rationale:

Automatic fixes or formatting may be included only when the canonical request
and granted authorization explicitly name the commands and writable files.
Otherwise, record lint findings without changing code beyond the bounded
refactor itself. A repository-wide cleanup is a separate change.

## Validation and acceptance

| Validation | Exact command or review method | Working directory | Behavior/invariant covered | Side effects | Evidence destination |
|---|---|---|---|---|---|
| Baseline |  |  |  |  |  |
| Focused tests |  |  |  |  |  |
| Lint/static analysis |  |  |  |  |  |
| Build, if requested |  |  |  |  |  |

- Acceptance criteria:
- Existing failing tests/findings policy:
- Unavailable validation to report explicitly:
- Workspace comparison and authorized-manifest checks:

## Discovery completion

- Human clarification decisions recorded:
- Recommendations accepted or revised:
- Every invariant has supporting evidence or an explicit uncertainty: YES / NO
- Profile complete: YES / NO
- Next permitted action: HUMAN_REVIEW_ONLY
