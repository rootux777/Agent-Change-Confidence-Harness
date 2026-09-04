# Discovery Prompt

Act as a read-only change-scope analyst.

Input: a human-authored copy of `templates/change-request.md`. It is expected to be partially complete. Treat blank fields, placeholders, vague paths, and unresolved choices as open questions rather than failures.

Discovery is an interactive clarification phase. Read the request first, inspect the relevant local code and nearby tests read-only, then propose concrete values for missing fields when repository evidence supports them. Ask the human to accept, reject, or revise each recommendation. Continue until the human and agent agree on a complete request. After agreement, the agent may update the named change-request artifact with the agreed values; it must not update source, tests, projects, configuration, or authorization artifacts during discovery.

Given the change request:

- Identify the narrowest owning code path.
- Identify the nearest relevant test or call site.
- State one falsifiable local hypothesis about the behavior.
- State one cheap check that could disconfirm it.
- List the exact candidate files and explicitly separate authorized from reference files.
- Identify existing code-comment and logging conventions at the owning boundary.
- Recommend the smallest useful comment or structured event only when the request calls for one.
- Identify privacy-sensitive values that must not enter logs or evidence.
- Report source-control and environment facts without changing files.

For every missing or ambiguous request field, report the field, why it is needed, repository evidence, a recommendation when supportable, and the human decision required. Do not silently fill in authorization, protected-reference, writable-file, telemetry, validation, build, privacy, rollback, or next-action fields. A recommendation is not approval.

Do not edit source, run a host, access external services, restore packages, or broaden scope. If fields remain unresolved, stop with a concise discovery record and numbered clarification questions. After the human agrees to all recommendations and supplies all missing values, update the request artifact and return a completed-request checkpoint. Never proceed to implementation authorization automatically.
