# Database Migration Change-Request Profile

Use this companion with a new, change-specific copy of `change-request.md` when
the selected change profile is `DATABASE_MIGRATION`. It converts accepted database
and data-contract review findings into a bounded migration proposal. The canonical
`change-request.md` remains the source of truth.

This profile is a proposal, not an execution grant. Completing or accepting it
does not authorize database access, DDL, DML, migration generation or application,
data reads or exports, backups, restores, application deployment, permission
changes, external-service access, or production activity.

## Request linkage and prerequisite gate

- Migration Change ID:
- Canonical change-request path:
- Human reviewer and decision-maker:
- Application governance playbook:
- Accepted database/data-contract review path:
- Accepted review Finding, Contract, Object, Flow, and Decision IDs:
- Review acceptance decision and date:
- Separate migration request justified because:
- External evidence directory:
- Request status: DRAFT / READY_FOR_HUMAN_SCOPE_REVIEW / COMPLETE_NOT_AUTHORIZED / SUPERSEDED

Do not reuse the review's Change ID or authorization. If no accepted review exists,
record the human-approved exception and the evidence that makes migration planning
safe. Never infer migration authority from a review finding or recommendation.

## Intended outcome and boundaries

- Business/operational outcome:
- Technical migration outcome:
- Source system/schema:
- Target system/schema:
- Environments proposed for later execution:
- Objects and data domains in scope:
- Applications, jobs, reports, integrations, and principals in scope:
- Explicit non-goals:
- Behavior and contracts that must remain unchanged:
- Data that must not be read, copied, logged, or retained:
- Requested human decision:

## Source, target, and ownership registry

| System ID | System/component | Role | Technology/version | Repository and protected reference | Database/schema identity | Migration mechanism | Runtime registration | Deployment owner | Evidence IDs |
|---|---|---|---|---|---|---|---|---|---|
| `SYS-001` | [NAME] | [SOURCE / TARGET / APPLICATION / ETL / REPORT / EXTERNAL] | [VERSION] | [PATH AND HEAD/HASH] | [REDACTED IDENTITY OR NOT_INSPECTED] | [TOOL/MANUAL/UNKNOWN] | [OBSERVED/ABSENT/UNKNOWN] | [ROLE OR UNKNOWN] | [DBE-###] |

- Authoritative desired-schema source:
- Authoritative migration history/head:
- Observed deployed-schema source, or NOT_INSPECTED:
- Known schema/context overlaps or exclusions:
- Migration and deployment ownership decision:

Keep source definitions, ORM expectations, migration history, generated scripts,
and deployed database metadata distinct. A model or migration file does not prove
that an environment contains the corresponding schema.

## Preconditions and unresolved decisions

| Decision ID | Required decision or precondition | Evidence/rationale | Options | Recommendation | Human owner | Blocking | Status |
|---|---|---|---|---|---|---|---|
| `DBD-001` | [QUESTION/PRECONDITION] | [EVIDENCE] | [OPTIONS] | [RECOMMENDATION] | [ROLE] | [YES/NO] | [OPEN/DECIDED/BLOCKED] |

All blocking decisions must be `DECIDED` and recorded in the canonical request
before implementation authorization is prepared. Examples include target tenant or
owner assignment, handling of unmapped data, compatibility window, acceptable
downtime, retention, rollback window, and the accountable cutover decision-maker.

## Migration strategy

- Strategy: EXPAND_BACKFILL_SWITCH_CONTRACT / IN_PLACE / SHADOW_COPY / REBUILD / OTHER
- Why this is the narrowest safe strategy:
- Expected migration units/batches:
- Ordering and dependency rationale:
- Online/offline expectation:
- Compatibility window:
- Point of no simple rollback:
- Cleanup/contract phase Change ID, or DEFERRED:

Prefer additive, backward-compatible stages when application versions may overlap.
Treat destructive cleanup as a later stage or separate change unless the human has
accepted its necessity, timing, evidence, and rollback limitations.

## Phase plan

| Phase ID | Phase | Intended state transition | Objects/data affected | Application dependency | Entry criteria | Exit evidence | Rollback/compensation | Separate authorization boundary |
|---|---|---|---|---|---|---|---|---|
| `DBM-P01` | [EXPAND / BACKFILL / VERIFY / SWITCH / CONTRACT] | [FROM -> TO] | [SCOPE] | [DEPENDENCY] | [CRITERIA] | [EVIDENCE] | [METHOD] | [BOUNDARY] |

Do not assume one authorization covers every phase. A phase that changes the
environment, application deployment, permissions, writers/readers, or destructive
scope requires its own explicit boundary and expiry conditions.

## Source-to-target data mapping

Create one row per migrated or deliberately excluded datum. Use stable Mapping IDs.

| Mapping ID | Source object/field | Target object/field | Contract ID | Source type/rules | Target type/rules | Transformation/default | Key/reference translation | Missing/invalid-data policy | Loss/archive decision | Evidence IDs |
|---|---|---|---|---|---|---|---|---|---|---|
| `DBMAP-001` | [SOURCE] | [TARGET] | [DC-###] | [RULES] | [RULES] | [TRANSFORMATION] | [METHOD] | [REJECT/QUARANTINE/DEFAULT/BLOCK] | [DECISION] | [DBE-###] |

Mapping rules:

- Never invent required business values merely to satisfy target constraints.
- Record identifier translations and their audit/reconciliation location without
  exposing sensitive row values.
- Identify companion/parent/child records that must be created in a defined order.
- State timezone, encoding, collation, case, normalization, precision, scale,
  rounding, enum, and null handling where relevant.
- Preserve unmapped source fields only under an approved classification, purpose,
  retention period, and access rule; otherwise record an explicit discard decision.
- Treat a default used for historical migration separately from the default for new
  application writes.

## Reader, writer, compatibility, and cutover matrix

| Consumer/writer ID | Application/job/report/principal | Before migration | During coexistence | After switch | Contract/version requirement | Deployment order | Rollback behavior | Owner | Evidence IDs |
|---|---|---|---|---|---|---|---|---|---|
| `DBC-001` | [NAME] | [READS/WRITES WHERE] | [BEHAVIOR] | [BEHAVIOR] | [REQUIREMENT] | [ORDER] | [BEHAVIOR] | [ROLE] | [IDS] |

- Dual-read or dual-write policy:
- Source-of-truth rule during coexistence:
- Conflict-resolution rule:
- Cache/search/index/report refresh dependencies:
- Event/CDC/replication compatibility:
- Application feature flag or switch mechanism:
- Cutover decision and observation window:
- Old-store retention and removal approval:

## Roles, permissions, secrets, and configuration

| Principal/config ID | Component or role | Environment | Required before | Required during | Required after | Least-privilege evidence | Rotation/removal step | Owner |
|---|---|---|---|---|---|---|---|---|
| `DBP-001` | [ROLE/CONFIG NAME] | [ENVIRONMENT] | [ACCESS] | [ACCESS] | [ACCESS] | [EVIDENCE] | [STEP] | [OWNER] |

- Connection/configuration change mechanism:
- Secret source and rotation policy (names only; never values):
- Seeded or embedded credential findings and disposition:
- Row/tenant security implications:
- Audit identity used during migration:
- Permission rollback:

Never place credentials, connection strings, raw secret values, personal data, or
unnecessary database host details in this request, commands, logs, or evidence.

## Proposed migration artifacts

List candidate artifacts during discovery. The later authorization must convert
them into an exact writable-file manifest.

| Artifact ID | Candidate path/object | Purpose | Generated or authored | Authoritative after acceptance | Rollback relevance | Evidence destination |
|---|---|---|---|---|---|---|
| `DBA-001` | [PATH/OBJECT] | [PURPOSE] | [GENERATED/AUTHORED] | [YES/NO] | [DETAILS] | [EXTERNAL PATH] |

Include, when applicable, migration source, reviewed/generated SQL, down or
compensation script, data transformation code, tests, query allowlist, dry-run
results, schema snapshots, reconciliation results, deployment/runbook instructions,
and source/target identity manifests.

## Proposed commands and database operations

Every row is a proposal until an applicable authorization names it exactly.

| Operation ID | Phase ID | Exact command or reviewed script/query ID | Working directory/environment | Purpose | Expected semantics | Reads | Writes/locks | Data exposure | Timeout/resource bound | Required role | Evidence destination |
|---|---|---|---|---|---|---|---|---|---|---|---|
| `DBOP-001` | `DBM-P01` | [COMMAND/ID] | [PATH/ENV] | [PURPOSE] | [RESULT/EXIT SEMANTICS] | [SCOPE] | [SCOPE] | [CLASSIFICATION] | [BOUND] | [ROLE] | [PATH] |

- Package restore/network requirement:
- Transaction boundary and vendor limitations:
- Retry/re-entry behavior:
- Idempotency or migration-history guard:
- Lock/table-rewrite/index-build expectation:
- Replication/CDC/trigger/job effects:
- Maintenance window or traffic controls:
- Stop conditions and accountable operator:

Generated SQL must be captured and reviewed when the selected tooling can emit or
execute operations not obvious from source migrations. A successful command exit
does not prove data completeness, application compatibility, or business outcome.

## Dry-run and rehearsal plan

- Approved non-production environment required:
- Representative schema/data basis:
- Sanitization or synthetic-data requirement:
- Starting identity/snapshot:
- Reset/repeat method:
- Migration duration/resource measures:
- Failure injection or interrupted-run checks:
- Application compatibility checks:
- Dry-run acceptance criteria:
- Evidence artifacts:

A rehearsal against an unrepresentative empty database establishes only schema
creation behavior. Record limitations before using it to support production
planning.

## Reconciliation and data-quality plan

Define validation during discovery; execute only exact authorized operations.

| Check ID | Phase | Invariant | Source population | Expected target population | Exact aggregate/assertion/query ID | Rejected/quarantined handling | Privacy/resource controls | Acceptance rule | Evidence artifact |
|---|---|---|---|---|---|---|---|---|---|
| `DBV-001` | [BEFORE/DURING/AFTER] | [ASSERTION] | [BOUNDED SET] | [BOUNDED SET] | [DBOP-###] | [POLICY] | [CONTROLS] | [RULE] | [PATH] |

Cover, where applicable:

- source eligibility, exclusions, and row counts;
- target counts and one-to-one/one-to-many cardinality;
- identifier/audit-map completeness;
- referential, uniqueness, nullability, check, and domain constraints;
- duplicates, rejected rows, defaults, truncation, precision, and normalization;
- application reads and writes against the intended version;
- old and new writer behavior during coexistence;
- permissions and tenant/ownership isolation;
- downstream ETL, reports, events, search, caches, and integrations;
- post-cutover monitoring without storing unnecessary row values.

A count match alone is insufficient when rows may be duplicated, misassigned,
truncated, defaulted, or linked to the wrong owner.

## Rollback, recovery, and point of no return

- Backup/snapshot requirement and owner:
- Backup verification/restore rehearsal evidence:
- Schema rollback or forward-fix strategy:
- Data rollback/compensation strategy:
- Writes occurring after migration starts:
- Reverse transformation feasibility:
- Audit mapping required for reversal:
- Rollback trigger and decision-maker:
- Rollback time objective:
- Point of no return:
- Retention/removal schedule for old objects:

Do not claim rollback is available solely because a down migration exists. State
how post-cutover writes, destructive transformations, external side effects, and
application-version compatibility affect recovery.

## Validation and acceptance

| Validation ID | Stage | Exact method/command/query ID | Behavior or risk covered | Expected result | Existing-findings policy | Evidence destination |
|---|---|---|---|---|---|---|
| `DBVAL-001` | [STATIC / GENERATION / DRY_RUN / CUTOVER / POST_CUTOVER] | [METHOD] | [COVERAGE] | [RESULT] | [POLICY] | [PATH] |

- Migration source/schema static validation:
- Focused migration and transformation tests:
- Generated-script review:
- Dry-run reconciliation:
- Application compatibility validation:
- Performance/resource/locking validation:
- Privacy/security/permission validation:
- Post-cutover smoke and observation window:
- Unavailable validation to report explicitly:
- Overall acceptance criteria:

## Authorization and publication boundaries

Before any implementation or execution, the human must define separate authority
for the applicable stage. At minimum, resolve:

- exact writable repository files and protected references;
- exact local generation, build, and test commands;
- exact database environment, principal, scripts/queries, objects, and operations;
- allowed data classification, output, retention, and redaction;
- resource, timeout, locking, and maintenance-window controls;
- application deployment, permission change, and external-service boundaries;
- evidence directory, expiry condition, stop conditions, and next permitted action.

The generic `implementation-authorization.md` does not by itself authorize
deployment or external database access. Any authorization intended to permit a
database operation must say so explicitly and must not rely on this request as the
grant. Production execution, where applicable, remains a distinct human-controlled
operation.

## Discovery completion

- Accepted review findings and decisions recorded:
- Candidate files/objects narrowed to exact scope:
- Mapping and compatibility questions resolved:
- Dry-run, reconciliation, rollback, and acceptance plans complete:
- Recommendations accepted or revised:
- Request complete: YES / NO
- Request status: COMPLETE_NOT_AUTHORIZED / NEEDS_MORE_EVIDENCE / BLOCKED
- Next permitted action: HUMAN_AUTHORIZATION_PREPARATION / HUMAN_REVIEW_ONLY

Completion confirms only that the migration proposal is reviewable. It does not
authorize implementation, database access, migration execution, application
deployment, permission changes, cleanup, publication, or production activity.
