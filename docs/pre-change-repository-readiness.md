# Pre-Change Repository Readiness Assessment

Use this read-only baseline before asking an agent to plan or implement a change in an existing repository. It gives the human and agent a shared view of the repository's architecture, conventions, controls, and history-derived risk signals without turning observations into unauthorized changes.

The readiness assessment is not a substitute for the harness change request. It happens before that workflow and never grants editing authority.

## Boundary and Flow

```text
Read-only readiness assessment
  -> human baseline review
  -> optional, separately authorized hardening change
  -> normal change request discovery
  -> separate implementation authorization
```

The target repository remains read-only during assessment. The report belongs in the external harness evidence directory, for example:

```text
<harness-path>/evidence/<project-name>/readiness/<assessment-id>/repository-readiness.md
```

Do not create an `evidence/` directory inside the target repository. The agent returns the report for the human to save unless the human explicitly authorizes writing that external artifact.

## Invoke the Assessment

Use this in a fresh, read-only agent session:

```text
Use <harness-path>/prompts/pre-change-repository-readiness.md.

The target repository is: <target-repository-path>.
Return the report for a human to save at:
<harness-path>/evidence/<project-name>/readiness/<assessment-id>/repository-readiness.md.

Use a Git-history window of: <for example, 180 days>.
Do not modify the target repository, run validation, access external services, or create the report file unless I explicitly authorize that external write.
```

Required inputs are the target repository path and report destination. Assessment ID, intended future change ID, time window, areas of concern, exclusions, and redaction needs are optional. Missing evidence remains `UNKNOWN`.

## How to Read the Result

Architecture statements have four distinct evidence labels:

| Label | Meaning |
| --- | --- |
| `OBSERVED` | Directly supported by inspected repository files. |
| `DOCUMENTED_BY_PROJECT` | Stated by project documentation or instructions. |
| `INFERRED` | A constrained interpretation; the report supplies evidence and confidence. |
| `UNKNOWN` | Evidence is unavailable or insufficient. |

Quality controls are also distinct from recommendations:

| Status | Meaning |
| --- | --- |
| `PRESENT_AND_ACTIVE` | Configuration and an active invocation path are evidenced. |
| `PRESENT_BUT_UNVERIFIED` | Configuration or documentation exists, but execution is not verified. |
| `ABSENT` | Relevant inspected locations show no control. |
| `UNKNOWN` | Evidence is unavailable or insufficient. |

Git-history results identify areas worth asking about, not code quality, ownership, expertise, or individual performance. A file's churn, contributor concentration, fix-related commits, or reverts are signals to investigate with the team and repository evidence.

`BLESSED_CONFIRMED` requires explicit project documentation. Every other plausible example remains `BLESSED_CANDIDATE` until a human confirms it.

## From Assessment to Authorized Work

At human baseline review, choose one of these paths:

- Accept the report as contextual evidence and begin a normal, bounded change request.
- Ask focused follow-up questions while keeping the repository read-only.
- Create a separate hardening change request for one incremental recommendation.
- Mark the assessment incomplete or blocked pending missing evidence.

Hardening—such as adding linting, CI, hooks, security scanning, or agent instructions—is a change. It requires a normal completed request and a separate implementation authorization. Do not bundle broad formatting or unrelated hardening into a functional change.

## Non-Goals and Stop Conditions

The assessment does not run tests, builds, package restores, hosts, formatters, linters, scanners, or generators. It does not create branches, commits, tags, hooks, pull requests, repository files, or external-service records. It does not fix findings.

Stop and report `UNKNOWN`, `BLOCKED_MISSING_EVIDENCE`, `BLOCKED_UNSAFE_REPOSITORY_STATE`, `NOT_A_GIT_REPOSITORY`, or `ASSESSMENT_INCOMPLETE` when the evidence or safe authority boundary is unavailable. Never return `READY_FOR_IMPLEMENTATION`.

## Reusable Report Template

```md
# Pre-Change Repository Readiness Report

- Assessment ID: <human-supplied or UNKNOWN>
- Timestamp: <ISO 8601 timestamp>
- Target repository: <path or REDACTED>
- Repository identity: <Git root, NOT_A_GIT_REPOSITORY, or UNKNOWN>
- HEAD: <immutable commit ID, UNKNOWN, or NOT_A_GIT_REPOSITORY>
- Assessment scope: <human-supplied scope and history window>
- Final status: <one required status>

## Authority Boundary

- Target repository modified: NO
- External report written: <YES / NO>
- Prohibited operations avoided: <YES / UNKNOWN>
- Access limitations and redactions: <list or NONE>

## Working-Tree State

- Branch or HEAD state: <value or UNKNOWN>
- Working tree: <CLEAN / DIRTY / UNKNOWN>
- State evidence: <concise command result>

## Repository and Technology Summary

| Topic | Finding | Evidence | Classification |
| --- | --- | --- | --- |
| Entry points |  |  | OBSERVED / DOCUMENTED_BY_PROJECT / INFERRED / UNKNOWN |
| Languages and frameworks |  |  |  |
| Build and package systems |  |  |  |
| Repository instructions |  |  |  |
| Generated or protected directories |  |  |  |

## Architecture Observations

| Area | Finding | Evidence | Classification | Confidence / unresolved question |
| --- | --- | --- | --- | --- |
| Modules and dependency direction |  |  |  |  |
| Data and external boundaries |  |  |  |  |
| Configuration |  |  |  |  |
| Error handling and logging |  |  |  |  |
| Validation and authorization |  |  |  |  |
| Tests and fixtures |  |  |  |  |

## Git-History Risk Signals

| Signal | Time window | Finding | Evidence | Interpretation limit |
| --- | --- | --- | --- | --- |
| Change concentration |  |  |  | Risk signal only |
| High-churn critical areas |  |  |  | Risk signal only |
| Contributor concentration |  |  |  | Not expertise or ownership |
| Fix/revert activity |  |  |  | Requires repository context |
| Development cadence |  |  |  | Not individual performance |

## Quality-Control Matrix

| Control | Status | Evidence | Verification limit | Follow-up needed |
| --- | --- | --- | --- | --- |
| Formatting |  |  |  |  |
| Linting |  |  |  |  |
| Type/static analysis |  |  |  |  |
| Test layers |  |  |  |  |
| Coverage |  |  |  |  |
| Security/dependency scanning |  |  |  |  |
| Hooks |  |  |  |  |
| CI and review controls |  |  |  |  |
| Agent instructions |  |  |  |  |

## Candidate Blessed Patterns

| Pattern | Status | Example paths | Supporting evidence | Conflicts | Confidence | Human decision needed |
| --- | --- | --- | --- | --- | --- | --- |
| Service/domain component | BLESSED_CONFIRMED / BLESSED_CANDIDATE / UNKNOWN |  |  |  |  |  |
| Validation |  |  |  |  |  |  |
| Error handling and logging |  |  |  |  |  |  |
| Data access |  |  |  |  |  |  |
| API/configuration/tests/DI/auth |  |  |  |  |  |  |

## Gaps and Proposed Gates

| Priority | Current condition | Evidence | Proposed control | Benefit | Adoption and broad-change risk | Expected files | Human authorization required |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |  |  |  |  |  |  | YES |

## Risks and Unresolved Questions

- <risk or UNKNOWN>

## Commands Executed

| Command purpose | Literal command or `NOT_RECORDED` | Concise result | State-changing risk |
| --- | --- | --- | --- |
|  |  |  | NONE |

## Recommended Next Action

<Human baseline review, focused read-only follow-up, or a separately authorized bounded hardening/change request.>

## Human Authorization Checkpoint

- Baseline reviewed by:
- Decision: ACCEPT_CONTEXT / REQUEST_FOLLOW_UP / AUTHORIZE_SEPARATE_CHANGE / BLOCKED
- No implementation authority granted by this report: YES
```
