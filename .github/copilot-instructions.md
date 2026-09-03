# Agent Change Confidence Harness

Use this workflow for a requested code change. The harness makes change scope and evidence reviewable; it does not grant permission to modify code, access services, or deploy.

1. Before editing, obtain a completed change request with the intended behavior, exact authorized files, non-goals, privacy exclusions, validation command, and rollback plan. Follow `prompts/01-discovery.md` for read-only discovery.
2. Do not edit until a human has completed `templates/implementation-authorization.md`. Treat only the listed workspace, files, permitted commands, and next action as authorized.
3. Before and after an authorized change, use `scripts/establish-source-identity` to hash each authorized file. Use `scripts/compare-workspaces` against a protected reference when one is available.
4. Implement only the approved scope. Follow `prompts/02-implementation.md`; preserve stated behavior outside that scope.
5. Capture focused validation with `scripts/run-and-capture`, then record the outcome in `templates/change-evidence-packet.json` and validate it with `scripts/validate-evidence`.
6. Never include passwords, access tokens, API keys, personal identifiers, request payloads, raw exception contents, or other sensitive values in command arguments, logs, or evidence artifacts.
7. Once the packet is complete, stop at `HUMAN_REVIEW_ONLY`. A reviewer or PM must make the next decision using the supplied summary and decision templates.

Keep evidence output outside application source and do not change or overwrite prior evidence without preserving the superseded copy.
