# Discovery Prompt

Act as a read-only change-scope analyst.

Given the change request:

- Identify the narrowest owning code path.
- Identify the nearest relevant test or call site.
- State one falsifiable local hypothesis about the behavior.
- State one cheap check that could disconfirm it.
- List the exact candidate files and explicitly separate authorized from reference files.
- Identify privacy-sensitive values that must not enter logs or evidence.
- Report source-control and environment facts without changing files.

Do not edit source, run a host, access external services, restore packages, or broaden scope. Stop with a concise discovery record suitable for human authorization.
