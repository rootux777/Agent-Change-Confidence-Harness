# Consequential-Action Inventory and Cross-System Reconciliation

Use this template to trace consequential actions within one or more application
components and reconcile evidence-supported relationships between them. Create a
project-specific copy under the external harness evidence directory. Do not edit
the reusable template while conducting an assessment.

This artifact records observed behavior; it does not authorize implementation,
runtime interaction, external-system access, package installation, builds, tests,
or publication.

## Assessment metadata

- Application name:
- Assessment ID:
- Human reviewer and decision-maker:
- External evidence directory:
- Assessment date:
- Assessment scope:
- Explicit exclusions:
- Prior readiness report:
- Prior entry-point report:
- Inventory version:
- Supersedes, or NOT_APPLICABLE:
- Current status: DRAFT / PARTIAL / READY_FOR_HUMAN_REVIEW / SUPERSEDED

## Authority boundary

- Target repositories are read-only.
- Do not edit, create, delete, rename, format, or generate repository files.
- Do not install or restore dependencies, start services, execute application
  code, invoke external systems, or publish messages or requests.
- Use existing read-only search and navigation tools such as `rg`, Git inspection,
  syntax-aware search, or language-server navigation when already available.
- Record commands and limitations. Do not claim complete coverage when a required
  tool, repository, generated source, runtime registration, or dependency is
  unavailable.
- Write this artifact or hash manifests only to the external evidence directory
  and only when the human has authorized those external writes.
- Never record secrets, tokens, cookies, private keys, personal information,
  complete sensitive payloads, or raw production data.

## Identity model

The inventory uses three independent identifiers:

- **Action ID** identifies a logical consequential action and remains stable when
  the inventory is revised.
- **Bridge ID** identifies a claimed relationship between two actions or system
  boundaries.
- **Evidence ID** identifies a cited source location. Its SHA-256 value fingerprints
  the exact file bytes inspected; the hash is not a permanent file or Action ID.

Never invent a hash, Action ID, Bridge ID, counterpart, or verification result.

## Component registry

Assign each component a short, stable uppercase prefix. Do not change a prefix
after Action IDs have been reviewed or referenced by another artifact.

| Component | Role | Repository root | Repository identity / HEAD | Branch and working-tree state | Action prefix | Instructions inspected | Coverage notes |
|---|---|---|---|---|---|---|---|
| [COMPONENT NAME] | [UI / API / SERVICE / WORKER / LIBRARY / EXTERNAL ADAPTER / OTHER] | [ABSOLUTE PATH] | [COMMIT, HASH MANIFEST, OR NOT_A_GIT_REPOSITORY] | [STATE] | [PREFIX] | [PATHS OR NONE] | [NOTES] |

Action IDs use `[PREFIX]-A###`, for example `WEB-A001`, `API-A001`, or
`PAY-A001`. Number actions sequentially within each component. Never renumber,
reuse, or reassign an accepted Action ID. New discoveries continue the existing
sequence.

## Evidence manifest

Create one row for every source file cited by an action or bridge. Reuse an
Evidence ID when multiple findings cite the same exact file identity. Use a new
Evidence ID after the file contents change, and mark dependent findings stale
until they are reverified.

| Evidence ID | Component | Repository-relative file | Symbol or lines | SHA-256 | Repository identity / HEAD | Working-tree status | Hash command and timestamp | Notes |
|---|---|---|---|---|---|---|---|---|
| `EV-001` | [COMPONENT] | [PATH] | [SYMBOL OR RANGE] | [COMPUTED HASH] | [IDENTITY] | [CLEAN / MODIFIED / UNTRACKED / UNKNOWN] | [COMMAND, UTC TIME] | [NOTES] |

Evidence rules:

1. Compute SHA-256 from the file actually inspected, not from a similarly named
   reference copy.
2. Record Git identity and working-tree status separately from the content hash.
   A Git commit does not prove that a dirty working copy matches that commit.
3. Hash every cited source file. Hash every file in the declared assessment scope
   only when full-scope drift detection is an explicit objective and the scope is
   precisely defined.
4. If only cited files were hashed, do not claim that uninspected files were
   protected or checked for tampering.
5. Recompute relevant hashes immediately before reconciliation. A mismatch makes
   dependent actions and bridges `STALE_EVIDENCE` until the changed source is read
   and evaluated again.
6. Store hash output outside application repositories. Report sensitive paths or
   remotes according to the project's redaction policy.

## Table A — Action and consequence

Create one row for each distinct action traced from an inbound trigger to a
state-changing or externally observable boundary.

| Action ID | Trigger | Caller | Path and symbol | Evidence IDs | Affected subject or resource | Classification | Immediate consequence | Related Action IDs |
|---|---|---|---|---|---|---|---|---|
| `[PREFIX]-A001` | `[TRIGGER TYPE]: [DETAIL]` | [ACTOR OR SYSTEM] | [PATH: SYMBOL] | `EV-###` | [RESOURCE AND HOW SELECTED] | [Record / Recommend / Act / Escalate] | [DIRECT EFFECT] | [ACTION IDs / UNRESOLVED / NONE] |

### Table A rules

1. **Trigger:** Begin with a project-defined trigger type such as `User Action`,
   `API Request`, `Event`, `Webhook`, `Job`, or `Internal Call`, followed by the
   concrete route, event, interaction, or job name.
2. **Caller:** Name only the initiating actor or system. Authentication and
   authority belong in Table B.
3. **Path and symbol:** Cite repository-relative paths and relevant symbols in
   traversal order. Every cited file must appear in the Evidence manifest.
4. **Affected subject or resource:** Identify the kind of record, account,
   message, payment, ticket, or other resource and how the specific instance is
   selected. Do not include sensitive values.
5. **Classification:** Use exactly one of:
   - `Record`: observes or persists facts without recommending or causing a
     consequential outcome;
   - `Recommend`: produces advice, a proposed choice, or a forecast;
   - `Act`: mutates material state or initiates an externally consequential
     instruction;
   - `Escalate`: routes an exception, approval, or human intervention.
6. **Immediate consequence:** State only what this path directly sends, writes,
   publishes, or changes. Do not infer business completion from an HTTP success,
   queue acknowledgement, database return, or similar transport result.
7. **Related Action IDs:** Use only relationships verified in the Bridge table.
   Use `UNRESOLVED` while reconciliation is pending and `NONE` only when
   investigation affirmatively confirms that no related action exists.

## Table B — Controls and outcome evidence

Every Table A row must have exactly one Table B row with the same Action ID.
Answer every control independently.

| Action ID | Authentication | Resource access | Action-specific authority | Input, state, and business-rule validation | External instruction and destination | Immediate transport or command result | Confirmed business outcome | Idempotency and duplicate protection | Reversibility or compensation | Audit and correlation evidence | Failure, retry, notification, and escalation owner | Confidence and unresolved question |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| `[PREFIX]-A001` | [EVIDENCE OR CONTROL TERM] | [EVIDENCE OR CONTROL TERM] | [EVIDENCE OR CONTROL TERM] | [OBSERVED GATES] | [INSTRUCTION AND DESTINATION] | [TRANSPORT RESULT ONLY] | [SEPARATE COMPLETION EVIDENCE] | [CONTROL] | [ACTION ID / UNRESOLVED / NONE] | [SAFE TRACE EVIDENCE] | [OBSERVED HANDLING AND OWNER] | [CONFIDENCE; QUESTION OR NONE] |

### Table B rules

1. Authentication does not establish access to the specific resource.
2. Resource access does not establish a mandate to perform the action.
3. Authentication and access together do not establish action-specific authority.
4. Instruction acceptance or transport success does not establish business
   completion.
5. Record input validation, state gates, and business-rule validation separately
   from identity and authority controls.
6. Record idempotency, reversibility, auditability, failure behavior, affected-user
   notification, and accountable escalation ownership only when directly evidenced.
7. Do not copy sensitive log values or payloads. Describe safe field names,
   correlation mechanisms, and redaction behavior instead.
8. State confidence and a concrete unresolved question. Use `NONE` only when no
   material question remains.

## Control vocabulary

Use these terms consistently:

- `Absent`: the relevant path was inspected and the control or evidence does not
  exist there.
- `Unknown`: the path was inspected, but available source cannot establish the
  answer, such as when runtime configuration or a third party determines it.
- `Not applicable`: the control has no meaningful application to this action;
  include the reason inline.
- `Not inspected`: the relevant path was not examined. Record it as a coverage gap.

## Initiation, confirmation, and compensation

Create separate Action IDs when initiation, completion confirmation, cancellation,
or compensation occur in different routes, consumers, callbacks, or jobs. Connect
them with verified Bridge rows. The initiating action must not claim a confirmed
business outcome that only another action observes.

## Cross-system Bridge table

Create a Bridge row for every claimed relationship between distinct actions or
boundaries. Do not infer a bridge from similar names, matching payload types, or
the existence of an endpoint alone.

| Bridge ID | Source Action ID | Destination Action ID | Relationship | Source evidence IDs | Destination evidence IDs | Contract or correlation evidence | Verification status | Confidence and unresolved question |
|---|---|---|---|---|---|---|---|---|
| `BR-001` | `[PREFIX]-A###` | `[PREFIX]-A###` | [CALLS / PUBLISHES_TO / CONSUMED_BY / CONFIRMS / COMPENSATES / OTHER] | `EV-###` | `EV-###` | [ROUTE, TOPIC, SCHEMA, IDENTIFIER, OR OTHER EVIDENCE] | [CANDIDATE / VERIFIED / REJECTED / UNRESOLVED / STALE_EVIDENCE] | [CONFIDENCE; QUESTION OR NONE] |

### Bridge verification rules

1. Inspect both sides of the relationship from current source.
2. Verify source registration and invocation, destination registration and
   handling, and the connecting route, destination, contract, or correlation key.
3. Cite Evidence IDs from both components. Recheck their hashes immediately before
   marking the bridge `VERIFIED`.
4. Record fan-out, fan-in, conditional routing, asynchronous delivery, and dynamic
   dispatch explicitly. Do not force a one-to-one relationship.
5. Use `CANDIDATE` for a plausible but incomplete link, `UNRESOLVED` when evidence
   cannot select a counterpart, `REJECTED` when inspection disproves a proposed
   link, and `STALE_EVIDENCE` after a relevant identity mismatch.
6. A `VERIFIED` bridge proves a source-level relationship, not successful runtime
   delivery or completed business outcome unless separately authorized runtime
   evidence establishes those facts.

## Reconciliation procedure

1. Confirm that each component's repository identity and working-tree state still
   match the inventory metadata.
2. Recompute SHA-256 for every file cited by the actions and bridges being
   reconciled.
3. Trace outbound relationships forward from each source action.
4. Trace inbound relationships backward from each destination action.
5. Compare route, method, destination, registration, contract fields, validation,
   identifiers, and outcome semantics.
6. Create or update Bridge rows only from evidence observed on both sides.
7. Update Table A Related Action IDs from `VERIFIED` Bridge rows only.
8. Record unmatched actions, ambiguity, contradictory assumptions, stale evidence,
   and coverage gaps without inventing a resolution.
9. Preserve the prior accepted inventory under `superseded/` before publishing a
   corrected version.

## Reconciliation summary

- Verified Bridge IDs:
- Candidate Bridge IDs:
- Rejected Bridge IDs:
- Unresolved Bridge IDs:
- Stale-evidence Bridge IDs:
- Unmatched source actions:
- Unmatched destination actions:
- Fan-out or fan-in relationships:
- Authentication/access/authority mismatches:
- Contract or validation mismatches:
- Initiation-without-confirmation findings:
- Confirmation-without-traced-initiation findings:
- Material human questions:

## Coverage and limitations

| Component | Paths inspected | Paths excluded | Paths not inspected | Dynamic/runtime uncertainty | Generated or unavailable source | Coverage assessment |
|---|---|---|---|---|---|---|
| [COMPONENT] | [PATHS] | [PATHS AND REASON] | [PATHS] | [DETAILS] | [DETAILS] | [COMPLETE_FOR_STATED_SCOPE / PARTIAL] |

- Commands and searches performed:
- Tools unavailable:
- Repository identity limitations:
- Hashing limitations:
- Runtime evidence authorized and sanitized, or NONE:
- Claims that remain source-only and not runtime-confirmed:

## Consistency checks

- Every Table A Action ID has exactly one Table B row: YES / NO
- Every Table B Action ID has exactly one Table A row: YES / NO
- Every cited source file has an Evidence manifest row: YES / NO
- Every recorded SHA-256 was computed from the stated file: YES / NO
- Every `VERIFIED` Bridge cites current evidence from both sides: YES / NO
- Every Table A related Action ID is supported by a `VERIFIED` Bridge: YES / NO
- Existing accepted Action, Bridge, and Evidence IDs remain stable: YES / NO
- Unresolved, unmatched, stale, and uninspected items are explicit: YES / NO
- No secrets or raw personal data are present: YES / NO
- Target repositories modified: NO / YES — EXPLAIN

## Human review

- Reviewer:
- Review date:
- Accepted scope and coverage:
- Required corrections:
- Decisions on unresolved questions:
- Inventory status: ACCEPTED / ACCEPTED_WITH_CHANGES / NEEDS_MORE_EVIDENCE / NOT_REVIEWED
- Next permitted action: HUMAN_REVIEW_ONLY / [EXPLICIT ACTION]

Acceptance of this inventory confirms only the recorded discovery and
reconciliation evidence. It does not authorize implementation, validation
execution, external-system access, commits, publication, merge, or deployment.
