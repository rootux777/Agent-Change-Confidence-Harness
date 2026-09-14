# Database and Data-Contract Review Profile

Use this companion with a change-specific copy of `change-request.md` when the
selected change profile is `DATABASE_DATA_CONTRACT_REVIEW`. The canonical change
request remains the source of truth. This profile reviews database definitions,
migration state, integrity controls, and the consistency of data contracts across
application layers.

This is a review profile, not authorization to change a schema. Ordinary
change-discovery authority may permit read-only inspection and search of repository
files, but completing this profile does not authorize analyzers, linters, schema
comparisons, database connections, queries, data extraction, source edits,
migration generation or application, DDL, DML, backup/restore, package
installation, commits, or deployment.

## Request linkage

- Change ID:
- Canonical change-request path:
- Human reviewer and decision-maker:
- Application governance playbook:
- Readiness, entry-point, and action-inventory evidence:
- External evidence directory:
- Review status: DRAFT / READY_FOR_AUTHORIZATION / READY_FOR_HUMAN_REVIEW / SUPERSEDED

## Review mode and intended decision

Select only the modes needed for the human decision:

- Review mode: STATIC_SCHEMA_REVIEW / MIGRATION_AND_DRIFT_REVIEW / DATA_CONTRACT_CONSISTENCY / DATA_QUALITY_REVIEW / COMBINED
- Human question this review must answer:
- Measurable review outcome:
- Systems and data domains in scope:
- Explicit exclusions:
- Findings disposition: RECORD_ONLY / MAY_SEED_SEPARATE_CHANGE_REQUESTS
- Production access requested: NO

`PRODUCTION access requested: NO` is the default. Any exception requires a new,
explicit human decision naming the environment, identity, exact metadata queries,
data minimization rules, timing/resource limits, evidence handling, and expiry.
Approval to inspect source code never implies approval to connect to a database.

## Authority level

Select one level. Higher levels include greater privacy and operational risk and
must not be inferred from a lower level.

| Level | Permitted activity | Separate implementation authorization required |
|---|---|---|
| `STATIC_SOURCE_ONLY` | Use ordinary read-only discovery to inspect schema files, migrations, ORM mappings, DTOs, API contracts, UI validation, ETL/report definitions, and configuration names without revealing secrets | No analyzer, linter, schema-build, or connection authority; external evidence writes still require human permission |
| `AUTHORIZED_LOCAL_TOOLING` | Run exact approved linters, parsers, schema-model builds, or offline comparisons against repository files | Yes — exact commands, working directories, caches, generated outputs, and network/restore policy |
| `AUTHORIZED_METADATA_ONLY` | Connect using an approved read-only identity and run only allowlisted catalog/schema metadata queries | Yes — exact environment, identity, queries, timeouts, output paths, and redactions |
| `AUTHORIZED_DATA_PROFILE` | Run bounded aggregate or assertion queries against approved non-production data | Yes — exact queries, columns, row/scan limits, privacy classification, retention, and resource controls |

- Selected authority level:
- Authorized environment, or NOT_APPLICABLE:
- Database identity/endpoint reference (redacted):
- Approved read-only principal or role (name only; never credentials):
- Authorization artifact path, or NOT_GRANTED:
- Authorization expiry condition:

## Database and system registry

| System ID | System/component | Role | Technology/version evidence | Repository or approved environment | Schema/database | Owner | Evidence IDs | Notes |
|---|---|---|---|---|---|---|---|---|
| `SYS-001` | [NAME] | [UI / API / DOMAIN / ORM / DATABASE / ETL / REPORT / FILE / QUEUE / EXTERNAL SERVICE] | [OBSERVED SOURCE] | [PATH OR REDACTED ENVIRONMENT] | [NAME OR NOT_APPLICABLE] | [TEAM/ROLE OR UNKNOWN] | `DBE-###` | [NOTES] |

Do not record connection strings, credentials, tokens, customer identifiers, or
sensitive host details. Use approved aliases or `REDACTED` when necessary.

## Evidence manifest

Use `DBE-###` IDs for exact evidence. Hash source artifacts with SHA-256 when they
support a finding. For connected metadata, record a sanitized query identity and
snapshot hash without including secrets or unnecessary application data.

| Evidence ID | System ID | Evidence type | Repository-relative path, object, or sanitized query ID | Symbol/range | Repository HEAD, migration head, or DB identity | SHA-256/snapshot hash | Working-tree or snapshot state | Captured at | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `DBE-001` | `SYS-001` | [SOURCE / MIGRATION / ORM / API / UI / ETL / REPORT / METADATA / PROFILE] | [REFERENCE] | [SYMBOL/RANGE] | [IDENTITY] | [HASH OR NOT_APPLICABLE] | [STATE] | [UTC TIME] | [NOTES] |

Evidence rules:

1. A Git commit identifies repository history; a SHA-256 value fingerprints exact
   bytes; a database identity identifies the inspected instance. None substitutes
   for the others.
2. Record dirty or generated source separately from committed source.
3. Never invent metadata or hashes. Mark unavailable evidence `Unknown` or
   `Not inspected`.
4. Recheck relevant identities before accepting a finding. Mark dependent findings
   `STALE_EVIDENCE` after a source, migration, or schema-identity mismatch.
5. Store only the minimum evidence necessary for review. Prefer counts, constraint
   names, types, and sanitized summaries over row values.

## Existing database controls

Inventory controls without assuming their presence proves correctness or active
deployment.

| Control area | Observed definition/configuration | Active invocation or enforcement evidence | Status | Evidence IDs | Limitation or question |
|---|---|---|---|---|---|
| Schema migrations |  |  | [PRESENT_AND_ACTIVE / PRESENT_BUT_UNVERIFIED / ABSENT / UNKNOWN] |  |  |
| Schema lint/static analysis |  |  |  |  |  |
| Schema drift detection |  |  |  |  |  |
| Foreign keys/referential integrity |  |  |  |  |  |
| Unique constraints |  |  |  |  |  |
| Check/domain constraints |  |  |  |  |  |
| Nullability/defaults/generated values |  |  |  |  |  |
| Indexes/performance controls |  |  |  |  |  |
| Triggers/stored procedures/views |  |  |  |  |  |
| Row/tenant security and permissions |  |  |  |  |  |
| Encryption/masking/auditing |  |  |  |  |  |
| Backup/restore or rollback evidence |  |  |  |  |  |
| Database tests |  |  |  |  |  |
| Data-quality assertions |  |  |  |  |  |

## Proposed tooling or queries

Discovery records proposals only. Do not run an item until its complete row is
copied into a granted `implementation-authorization.md`.

| Item ID | Tool/command or sanitized query | Working directory or environment | Purpose | Expected exit/result semantics | Reads | Writes/caches/locks | Data exposure | Timeout/resource bound | Evidence destination |
|---|---|---|---|---|---|---|---|---|---|
| `DBQ-001` | [EXACT COMMAND OR QUERY ID] | [PATH/ENVIRONMENT] | [PURPOSE] | [SEMANTICS] | [OBJECTS/COLUMNS] | [SIDE EFFECTS] | [NONE/METADATA/AGGREGATE/SENSITIVE] | [BOUND] | [EXTERNAL PATH] |

Connected-query rules:

- Use a least-privilege, read-only principal verified by the human or database
  owner. Do not assume a query is read-only merely because it starts with `SELECT`.
- Allowlist exact queries. Prohibit dynamic or agent-generated query expansion
  after authorization.
- Prefer metadata/catalog queries before data profiling. Prefer non-production,
  aggregates, row limits, statement timeouts, and approved replicas where valid.
- Account for query plans, functions, views, locks, temporary objects, audit logs,
  result caching, and vendor-specific side effects.
- Do not use `SELECT *`, export raw tables, retrieve secrets, or store row-level
  personal data in evidence.
- `EXPLAIN`, query-plan capture, migration validation, and schema comparison may
  consume resources or execute code depending on the database/tool. Record their
  actual semantics before authorization.

## Schema and migration consistency

Keep desired definitions, migration history, ORM expectations, and observed
database state distinct.

| Object ID | Object/field | Desired schema source | Migration representation | ORM/application representation | Observed DB metadata | Drift status | Evidence IDs | Question or consequence |
|---|---|---|---|---|---|---|---|---|
| `DBO-001` | [SCHEMA.OBJECT.FIELD] | [TYPE/CONSTRAINT] | [MIGRATION] | [MAPPING] | [METADATA OR NOT_INSPECTED] | [ALIGNED / CONTRACT_DRIFT / SCHEMA_DRIFT / MIGRATION_GAP / UNKNOWN / STALE_EVIDENCE] | [IDS] | [DETAILS] |

Review, where applicable:

- migration ordering, repeatability, checksums, baselines, and environment history;
- objects present without migrations and migrations not reflected in expected state;
- destructive or narrowing changes, implicit conversions, default/backfill order,
  nullability transitions, and constraint validation;
- rename versus drop/create behavior and dependency discovery;
- transactional limitations, locks, table rewrites, index builds, long-running
  backfills, replication/CDC impact, and rollback feasibility;
- ORM-generated names, conventions, shadow properties, concurrency fields, and
  migration snapshots;
- views, procedures, functions, triggers, jobs, reports, ETL, exports, and external
  consumers that may not be visible to ordinary dependency discovery.

## Data-contract definitions

Use stable `DC-###` IDs for semantic data elements. A contract describes meaning,
not merely a shared field name.

| Contract ID | Canonical semantic name | Business meaning | System of record | Classification | Required invariants | Lifecycle/retention | Owner | Evidence IDs | Open question |
|---|---|---|---|---|---|---|---|---|---|
| `DC-001` | [DOMAIN.FIELD] | [MEANING] | [SYSTEM] | [PUBLIC / INTERNAL / CONFIDENTIAL / PERSONAL / SENSITIVE / UNKNOWN] | [FORMAT/RANGE/NULLABILITY/UNIQUENESS/REFERENTIAL RULES] | [RULE] | [ROLE OR UNKNOWN] | [IDS] | [QUESTION OR NONE] |

## Contract representations

Create one `DR-###` row for every representation of a contract across UI, API,
domain, ORM, database, queue/event, file, ETL, report, or external service.

| Representation ID | Contract ID | System/layer | Path, object, and field | Type | Nullability/default | Length/precision/range | Format/domain | Validation/enforcement | Transformation | Evidence IDs | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|
| `DR-001` | `DC-001` | [UI/API/DOMAIN/ORM/DB/EVENT/FILE/ETL/REPORT/EXTERNAL] | [REFERENCE] | [TYPE] | [RULE] | [RULE] | [RULE] | [CONTROL] | [MAPPING/NORMALIZATION] | [IDS] | [OBSERVED / DOCUMENTED / INFERRED / UNKNOWN / STALE_EVIDENCE] |

Do not conclude that similarly named fields share a contract without evidence of
their mapping and meaning. Conversely, record renamed or transformed fields when
evidence shows they represent the same datum.

## Data-flow consistency

Use `DF-###` IDs to connect verified representations.

| Flow ID | Contract ID | Source representation | Destination representation | Transport/mapping | Validation before transfer | Error/rejection behavior | Loss or coercion risk | Evidence IDs | Verification status |
|---|---|---|---|---|---|---|---|---|---|
| `DF-001` | `DC-001` | `DR-###` | `DR-###` | [MAPPING] | [CONTROL] | [BEHAVIOR] | [TRUNCATION/ROUNDING/TIMEZONE/ENCODING/ENUM/NULL/OTHER] | [IDS] | [CANDIDATE / VERIFIED / REJECTED / UNRESOLVED / STALE_EVIDENCE] |

Inspect both sides before marking a flow `VERIFIED`. Static evidence can verify a
code/schema mapping; it does not prove successful runtime delivery, deployed
schema state, or data quality unless separately authorized evidence establishes it.

## Contract consistency matrix

| Contract ID | UI | API | Domain | ORM | Database | ETL/event/file | Report/external | Result | Conflicts |
|---|---|---|---|---|---|---|---|---|---|
| `DC-001` | [SUMMARY] | [SUMMARY] | [SUMMARY] | [SUMMARY] | [SUMMARY] | [SUMMARY] | [SUMMARY] | [ALIGNED / CONTRACT_DRIFT / UNKNOWN / STALE_EVIDENCE] | [DETAILS] |

Examples of contract drift include:

- one layer permits null while a downstream layer forbids it;
- valid upstream length, precision, scale, range, enum, timezone, encoding, or
  format cannot be represented downstream;
- normalization or validation occurs after a state-changing boundary rather than
  before it;
- a database constraint is stricter or weaker than the business/API contract;
- migration, ORM model, deployed metadata, ETL, report, or external contract
  represent incompatible versions;
- tenant, ownership, retention, classification, or masking rules are lost between
  layers.

## Data-quality assertions

Define assertions during discovery. Execute only exact authorized assertions
against an approved environment.

| Assertion ID | Contract ID/object | Invariant | Static evidence | Proposed aggregate/query ID | Expected result | Privacy/resource controls | Actual result artifact | Status |
|---|---|---|---|---|---|---|---|---|
| `DQA-001` | [REFERENCE] | [ASSERTION] | [IDS] | `DBQ-###` | [EXPECTATION] | [CONTROLS] | [PATH OR NOT_RUN] | [PROPOSED / PASS / FAIL / BLOCKED / NOT_RUN] |

A passing sample or aggregate supports only the stated assertion, environment, and
time. It does not prove all historical or future data is valid.

## Findings

Use stable `DBF-###` IDs. Findings are evidence for human review, not automatic
authorization to fix, migrate, reformat, backfill, or delete anything.

| Finding ID | Category | Affected IDs/objects | Evidence IDs | Observed condition | Potential consequence | Confidence | Uncertainty | Recommended next decision |
|---|---|---|---|---|---|---|---|---|
| `DBF-001` | [STATIC_QUALITY / SCHEMA_DRIFT / MIGRATION_GAP / CONTRACT_DRIFT / DATA_QUALITY / INTEGRITY / ACCESS / PRIVACY / PERFORMANCE / OPERABILITY] | [IDS] | [IDS] | [FACT] | [BOUNDED CONSEQUENCE] | [HIGH/MEDIUM/LOW] | [QUESTION OR NONE] | [RECORD / MORE_EVIDENCE / SEPARATE_CHANGE_REQUEST] |

Avoid unsupported severity labels and production-impact claims. Distinguish a
demonstrated defect, a static risk, a deployed-state uncertainty, and a policy
question.

## Review summary

- Database controls observed:
- Schema/migration alignment result:
- Contract IDs reviewed:
- Verified and unresolved data-flow IDs:
- Data-quality assertions executed, or NOT_RUN:
- Highest-confidence findings:
- Stale or unavailable evidence:
- Privacy and operational limitations:
- Recommended separately scoped follow-up requests:
- Explicit non-goals retained:

## Consistency and safety checks

- Every finding cites current Evidence IDs: YES / NO
- Every data flow references defined Contract and Representation IDs: YES / NO
- Static source, expected schema, migration history, and observed DB state remain distinct: YES / NO
- Connected activity, if any, exactly matches a granted authorization: YES / NO / NOT_APPLICABLE
- No DDL, DML, migration application, or schema mutation occurred: YES / NO — EXPLAIN
- No unapproved production access occurred: YES / NO — EXPLAIN
- No secrets or unnecessary row-level personal data appear in evidence: YES / NO
- Query/resource limitations are recorded: YES / NO / NOT_APPLICABLE
- Application repositories modified: NO / YES — EXPLAIN
- Database state modified: NO / YES — EXPLAIN

## Discovery completion

- Human clarification decisions recorded:
- Recommendations accepted or revised:
- Profile complete: YES / NO
- Review result: READY_FOR_AUTHORIZATION / READY_FOR_HUMAN_REVIEW / NEEDS_MORE_EVIDENCE / BLOCKED
- Next permitted action: HUMAN_REVIEW_ONLY / [EXPLICIT ACTION]

Acceptance of this review confirms only its recorded evidence and limitations. A
database or data-contract remediation requires its own completed change request,
exact migration and rollback design, separate implementation authorization,
validation evidence, and human decision.
