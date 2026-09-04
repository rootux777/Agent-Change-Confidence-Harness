# Pre-Change Application Entry-Point Mapping

Use this read-only mapping action after the Repository Readiness Assessment when a future change may cross UI, API, service, queue, worker, or scheduled-job boundaries. It turns architecture context into a traceable map of actual entry points and connections without sending traffic, starting services, or modifying the target repository.

It is contextual evidence, not a change request or implementation authorization.

## Boundary and Flow

```text
Read-only repository readiness assessment
  -> human baseline review
  -> read-only application entry-point mapping
  -> human entry-point review
  -> normal bounded change request discovery
  -> separate implementation authorization
```

The entry-point report belongs in the external harness, for example:

```text
<harness-path>/evidence/<project-name>/readiness/<assessment-id>/application-entry-points.md
```

Do not create an `evidence/` directory or any report file in the target repository. The mapping agent returns the report for the human to save unless the human explicitly authorizes the external report write.

## Invoke the Mapping

Use a fresh, read-only agent session:

```text
Use <harness-path>/prompts/pre-change-application-entry-points.md.

The target repository is: <target-repository-path>.
The completed readiness report is:
<harness-path>/evidence/<project-name>/readiness/<assessment-id>/repository-readiness.md.
Return the report for a human to save at:
<harness-path>/evidence/<project-name>/readiness/<assessment-id>/application-entry-points.md.

Do not modify the target repository, start services, access external systems, submit requests or messages, or create the report file unless I explicitly authorize that external write.
```

Optional inputs include an intended future change ID, focus area, excluded directories, redaction requirements, and sanitized runtime observations from a developer-authorized development or test environment.

## Evidence and Confidence

The mapping reports evidence, not assumptions:

| Status | Meaning |
| --- | --- |
| `CANDIDATE` | Located through text or syntax-aware search. |
| `REGISTERED` | Connected to startup, routing, dependency injection, or broker configuration. |
| `REFERENCED` | Symbol/call analysis links it upstream or downstream. |
| `RUNTIME_OBSERVED` | Seen in an explicitly authorized interaction. |
| `CONFIRMED_CHAIN` | Trigger, registration, handler, and material downstream action are linked by evidence. |
| `UNKNOWN` | Evidence is unavailable or insufficient. |

A name occurring in multiple files is not proof of an active path. The report may include only sanitized developer-supplied or explicitly authorized runtime facts; it must not retain authentication data, cookies, sensitive payload values, or complete HAR files.

If the readiness report's target repository or immutable `HEAD` does not match the current target, refresh the readiness baseline before relying on the map.

## From Mapping to Change Work

At entry-point review, the human may:

- Accept the map as context for a normal bounded change request.
- Request a focused read-only follow-up for a missing connection.
- Provide sanitized, authorized development/test runtime observations.
- Request a separate hardening or observability change through the normal request and authorization workflow.

The report cannot authorize source changes, runtime interaction, test messages, configuration changes, or deployment.

## Reusable Report Template

```md
# Pre-Change Application Entry-Point Report

- Assessment ID: <human-supplied or UNKNOWN>
- Timestamp: <ISO 8601 timestamp>
- Target repository: <path or REDACTED>
- Repository identity: <Git root or UNKNOWN>
- Branch or HEAD state: <value or UNKNOWN>
- HEAD: <immutable commit ID or UNKNOWN>
- Working tree: <CLEAN / DIRTY / UNKNOWN>
- Readiness Assessment reference: <path and matching HEAD, or UNKNOWN>
- Assessment focus: <human-supplied scope or ALL DISCOVERABLE ENTRY POINTS>
- Final status: <one required status>

## Authority Boundary

- TARGET_REPOSITORY_MODIFIED: NO
- External report written: <YES / NO>
- Runtime interaction performed: <NO / authorized sanitized observation only>
- Access limitations and redactions: <list or NONE>

## API Entry Points

| Method/operation | Route or operation | Handler | Source file | Payload type | Auth/validation | First downstream call | Registration evidence | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  | CANDIDATE / REGISTERED / REFERENCED / RUNTIME_OBSERVED / CONFIRMED_CHAIN / UNKNOWN |

## UI Entry Points

| Page/component | Control or event | Handler | Source file | API/service call | Expected payload shape | Result | Evidence | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |

## Queue, Worker, and Scheduled-Job Entry Points

| Destination/job | Consumer or worker | Source file | Message type | Registration | Publisher locations | Downstream action | Retry/failure behavior | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |

## Confirmed and Candidate Chains

### <Chain name>

- Starting trigger:
- Intermediate symbols and paths:
- API route or message destination:
- Final handler:
- Material side effect:
- Evidence for each connection:
- Missing connection:
- Overall status and confidence:

## Sanitized Runtime Evidence

| Observation source | Method/path or destination | Initiating symbol | Safe field names/status | Correlation/trace ID | Authorization and redaction note |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## Unknowns and Targeted Questions

- <Only questions that materially improve the map.>

## Commands and Searches Performed

| Purpose | Command or search pattern | Concise result | Limitation/state-changing risk |
| --- | --- | --- | --- |
|  |  |  | NONE |

## Recommended Next Action

<Human entry-point review, a focused read-only follow-up, or normal bounded change-request discovery.>

## Human Authorization Checkpoint

- Reviewed by:
- Decision: ACCEPT_CONTEXT / REQUEST_FOLLOW_UP / AUTHORIZE_SEPARATE_CHANGE / BLOCKED
- No implementation or runtime authority granted by this report: YES
```
