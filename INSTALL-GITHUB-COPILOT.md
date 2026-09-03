# Install With GitHub Copilot

## Add To A Local Project

Copy this package into a local, non-production workspace or add it as a local team tool directory. Keep the package outside application source and keep evidence output separate from application files. Do not publish or install it as a runtime dependency.

Example:

```sh
cp -R /path/to/Agent-Change-Confidence-Harness ./tools/Agent-Change-Confidence-Harness
```

**Verified in V0.1:** the package scripts use local shell commands, accept paths containing spaces when quoted, and do not require network access. The example packet validates with the `jsonschema` command available in the validation environment.

**Version-dependent guidance:** GitHub Copilot instruction discovery and repository customization locations can vary by editor, extension version, and team policy. Place prompts where your approved Copilot workflow can reference them; verify discovery behavior in the current editor before relying on it.

## Supply A Change Request

Start with `templates/change-request.md`. State the intent, exact authorized files, non-goals, privacy exclusions, validation command, and rollback. Use fictional or generic identifiers in shared examples.

## Authorize Implementation

Complete `templates/implementation-authorization.md` separately from the request. A human must explicitly grant implementation authority for the listed workspace and files. Do not treat a prompt, issue, or agent assumption as approval.

## Review Evidence

Ask the agent to:

1. Establish before-change hashes for every authorized file.
2. Record the literal commands and their results.
3. Capture focused validation and an authorized-file comparison.
4. Produce `templates/change-evidence-packet.json` using the schema.
5. Preserve superseded evidence before any evidence-only correction.
6. Stop at `HUMAN_REVIEW_ONLY` unless a human grants a new boundary.

Review source identity first, then changed files, test/build results, privacy fields, unavailable validation, uncertainties, and the human decision template. Treat expected nonzero search/diff outcomes separately from failed command execution.

## Remove Or Update

To remove the package, delete only the copied package directory and any evidence directory created for it after confirming your team has retained required records. To update it, replace the package directory with a reviewed version and record the package version in new evidence packets. Do not mutate old evidence packets in place without preserving the superseded copy.

**Not verified:** exact Copilot UI steps, repository instruction discovery, and editor-specific integration are version-dependent and must be checked against the current team setup.
