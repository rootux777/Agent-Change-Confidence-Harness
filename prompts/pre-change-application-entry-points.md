# Pre-Change Application Entry-Point Mapping Prompt

Act as a read-only application entry-point mapper for an unfamiliar target repository.

This follow-on assessment uses a completed Repository Readiness Assessment as context. Its purpose is to map evidence-supported paths through the application before a future agent proposes or implements a change. It never authorizes implementation, runtime interaction, repository hardening, or deployment.

## Inputs Required From the Human

- Target repository path.
- Completed readiness-report path.
- External harness report destination.

## Optional Inputs

- Assessment ID, intended future change ID, and focus area.
- Authorized sanitized runtime observations from a development or test environment.
- Directories to exclude, redaction requirements, and the entry-point types to prioritize.

If an input, tool, registration link, or runtime evidence is unavailable, record `UNKNOWN`; do not infer it.

## Authority Boundary

The target repository is read-only. Do not:

- Modify, format, create, rename, or delete repository files.
- Install tools or dependencies; restore packages; change configuration; or generate files.
- Start services; connect to external systems; submit API requests, UI forms, or queue messages; or inspect production systems without explicit human authorization.
- Create branches, commits, tags, hooks, pull requests, or source-control state.
- Record secrets, authentication tokens, cookies, personal information, complete sensitive payloads, or complete HAR files.
- Claim an entry point is active without sufficient registration or authorized runtime evidence.

You may use existing repository search, `rg` or an equivalent search tool, already-installed syntax-aware tools, language-server/IDE navigation, and existing read-only framework inspection only when it needs no installation or generated changes. If a tool is unavailable, continue with available methods and record the limitation.

Do not create files in the target repository. Return the Markdown report for the human to save in the supplied external harness evidence directory. Only create that external report file if the human explicitly authorizes that write.

## Assessment Procedure

### 1. Validate Readiness Context

Read the target repository instructions and the readiness report. Compare the report's repository identity and `HEAD` with the current target repository when both are available.

- If the target repository or immutable `HEAD` differs, record the mismatch and return `BLOCKED_MISSING_REPOSITORY_EVIDENCE` unless the human explicitly authorizes a refreshed readiness assessment.
- Record the current branch/detached state and working-tree state without changing them.
- Use the readiness report's technology, architecture, and protected-directory observations as context, but retain only conclusions supported by current repository evidence.

### 2. Map API and Service Entry Points

Search for HTTP routes, controller attributes/annotations/decorators, route registration, minimal or function-based endpoints, GraphQL resolvers, webhook receivers, RPC/gRPC definitions, OpenAPI/Swagger/protobuf contracts, request models, validation, authorization, and the first service or domain call.

For each discovered item, record operation type, route/operation, handler symbol, source file, payload type, evident authentication/authorization, validation entry point, first downstream call, registration evidence, and any authorized runtime evidence.

### 3. Map UI Entry Points

Search for button and form handlers, DOM or framework event bindings, mutation hooks, API-client calls, navigation actions, state changes, and pre-submission validation.

For each discovered item, record page/route/component, visible control or form, event type, handler symbol, source file, API-client or service call, expected payload shape, result/state/navigation/user feedback, and any authorized runtime observation.

When the human supplies or authorizes runtime observations, retain only sanitized method/path, initiating symbol, payload field names, response status, and safe correlation/trace identifiers. Do not preserve authentication headers, cookies, sensitive values, or unsanitized HAR files.

### 4. Map Queues, Workers, and Scheduled Work

Search for queues, topics, subscriptions, consumer registrations, listener annotations/decorators, workers/hosted services, scheduled jobs, handler interfaces, publishers/producers, retry/dead-letter/acknowledgement/failure behavior, and repository infrastructure definitions.

For each discovered item, record destination/job, consumer/worker symbol, registration, handler source file, message type/schema, publisher locations when found, downstream service/side effect, retry/failure behavior, and authorized runtime evidence.

Do not publish test messages, inspect production queues, or connect to a broker without explicit human authorization.

### 5. Connect Only Supported Chains

Where evidence permits, map a chain such as:

```text
UI trigger -> event handler -> API client -> API route -> controller/handler
-> service -> queue publisher -> destination -> consumer -> resulting action
```

Do not assume every repository has every stage. For every connection, cite symbols and repository-relative paths, state missing links, and record the material side effect only when evidenced.

Assign exactly one status to each discovered item and chain:

- `CANDIDATE`: located through text or syntax-aware search.
- `REGISTERED`: connected to startup, routing, dependency injection, or broker configuration.
- `REFERENCED`: symbol/call analysis links it upstream or downstream.
- `RUNTIME_OBSERVED`: observed processing an explicitly authorized interaction.
- `CONFIRMED_CHAIN`: trigger, registration, handler, and material downstream action are connected by evidence.
- `UNKNOWN`: evidence is unavailable or insufficient.

Do not raise confidence merely because a name appears in multiple files.

## Required Output

Return a Markdown report using the template in `docs/pre-change-application-entry-points.md`. Include:

1. Assessment reference, readiness-report reference, current repository identity, `HEAD`, and working-tree state.
2. Authority boundary and access/runtime limitations.
3. API, UI, and queue/worker entry-point tables.
4. Confirmed and candidate chains with evidence for each connection.
5. Sanitized authorized runtime evidence, if any.
6. Unknowns and targeted human questions.
7. Commands/searches performed, their concise results, and limitations.

End with exactly one status:

- `READY_FOR_ENTRY_POINT_REVIEW`
- `PARTIAL_ENTRY_POINT_MAP`
- `BLOCKED_MISSING_REPOSITORY_EVIDENCE`
- `BLOCKED_RUNTIME_AUTHORIZATION_REQUIRED`
- `NO_RELEVANT_ENTRY_POINTS_FOUND`

Also state `TARGET_REPOSITORY_MODIFIED: NO`. Never return `READY_FOR_IMPLEMENTATION`.
