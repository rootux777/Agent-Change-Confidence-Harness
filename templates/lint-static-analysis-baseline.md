# Lint and Static-Analysis Baseline Profile

Use this companion with a change-specific copy of `change-request.md` when the
selected change profile is `LINT_STATIC_ANALYSIS_BASELINE`. The canonical change
request remains the source of truth. This profile records the extra detail needed
to run existing checks without allowing their findings to expand the scope.

Completing this profile does not authorize command execution, source edits,
tool installation, package restore, network access, or automatic fixes.

## Request linkage

- Change ID:
- Canonical change-request path:
- Readiness report and recommendation ID:
- Repository root:
- Protected reference and working-tree state:
- Human reviewer:

## Baseline objective

- Existing lint/static-analysis control being measured:
- Why this control is the narrowest useful baseline:
- Measurable outcome:
- Check-only assertion: NO SOURCE EDITS REQUESTED
- Permitted evidence/output paths outside the application workspace:

## Observed tool configuration

- Tool and version source (lockfile, manifest, config, or installed executable):
- Configuration file paths:
- Included paths or projects:
- Excluded paths or projects:
- Existing scripts or CI commands that invoke the control:
- Material uncertainties:

## Proposed execution

Record one row per command. Commands remain proposals until copied into a
granted `implementation-authorization.md`.

| Command | Working directory | Purpose | Expected exit semantics | Prerequisites | Writes, caches, or generated files | Network or restore needed |
|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |

## Findings policy

- Baseline recording format and evidence destination:
- Treatment of pre-existing findings:
- Permitted new findings: NONE / specify an explicit threshold and rationale
- Tool/configuration errors versus code findings:
- Truncation or suppression handling:
- Secrets and personal-data exclusions:
- Acceptance criteria:

## Scope boundaries

- Automatic fix mode: PROHIBITED
- Broad formatting: PROHIBITED
- Source or configuration edits: PROHIBITED
- Dependency or tool installation: PROHIBITED unless separately requested and authorized
- Package restore: PROHIBITED unless separately requested and authorized
- Unrelated checks and cleanup: PROHIBITED
- Additional non-goals:

If the existing command cannot run without a narrowly scoped environment fix,
record the blockage. Do not convert the baseline into a control-addition,
configuration-change, formatting, or refactoring task. Those require a new or
amended request with separate authorization.

## Discovery completion

- Human clarification decisions recorded:
- Recommendations accepted or revised:
- Profile complete: YES / NO
- Next permitted action: HUMAN_REVIEW_ONLY
