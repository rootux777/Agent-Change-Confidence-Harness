# Evidence Repair Prompt

Perform evidence-only correction after human classification.

Do not modify source, tests, projects, configuration, or accepted implementation artifacts.

1. Preserve the current evidence package under a clearly named `superseded/` directory.
2. Rebuild identity artifacts from the protected reference or current authorized files as specified by the human decision.
3. Use a disposable copy for baseline validation; never generate output in a protected reference.
4. Record literal commands, working directories, timestamps, exit codes, and raw results.
5. Treat expected nonzero searches and comparisons as data, not execution failures.
6. Run a fail-closed validator covering every correction requirement.
7. Update claims to match evidence, including schema-validation scope and provider-redaction uncertainty.
8. Rehash authorized files after every correction and stop on identity mismatch.

Return only the corrected evidence status and the next permitted human action.
