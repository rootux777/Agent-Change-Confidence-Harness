# Use the Harness in VS Code

This guide is for developers using VS Code with GitHub Copilot Chat. The harness guides a change workflow; it does not automatically authorize edits or replace human review.

## One-Time Workspace Setup

1. Copy this package into the repository as `tools/Agent-Change-Confidence-Harness`.
2. Copy `.github/copilot-instructions.md` from this package to the application's `.github/copilot-instructions.md`. If the application already has that file, merge the harness rules without removing its existing project-specific instructions.
3. Create an evidence directory outside application source, such as `evidence/CHANGE-123/`, and add it to the application's ignore rules if evidence should not be committed.
4. Verify the local prerequisites:

```sh
bash --version
jq --version
shasum --version
jsonschema --version
```

In Copilot Chat, confirm that `.github/copilot-instructions.md` appears in the response's **References** list. This confirms VS Code supplied the repository instructions to Copilot.

## Per-Change Workflow

1. Complete `tools/Agent-Change-Confidence-Harness/templates/change-request.md`. Include the exact files that may be changed, explicit non-goals, approved validation, privacy exclusions, rollback instructions, and any requested comments or application logging/telemetry.
2. Open Copilot Chat in **Ask** mode. Attach or reference the completed request and `tools/Agent-Change-Confidence-Harness/prompts/01-discovery.md`. Ask Copilot to produce the discovery record only. Do not allow edits in this phase.
3. Have a human review the discovery record and complete `templates/implementation-authorization.md`. The authorization must name the writable workspace, allowed files, commands, prohibited operations, and next permitted action.
4. Open a new Copilot Chat session in the mode your team permits for edits. Attach or reference the authorization and `prompts/02-implementation.md`. Copilot may edit only after this approval is complete.
5. Ask Copilot to use the harness scripts to hash the authorized files, capture the approved validation, compare the protected reference with the workspace, and complete the evidence packet.
6. Review the generated packet, summary, validation logs, and uncertainties. Stop at `HUMAN_REVIEW_ONLY` until a reviewer or PM records the next decision.

## Recommended Copilot Chat Prompts

Use a prompt like this for discovery:

```text
Use tools/Agent-Change-Confidence-Harness/prompts/01-discovery.md.
The completed change request is at: <path>.
Remain read-only and return the discovery record.
```

Use this only after human authorization:

```text
Use tools/Agent-Change-Confidence-Harness/prompts/02-implementation.md.
The signed implementation authorization is at: <path>.
Work only within that authorization. Capture the required evidence and stop at HUMAN_REVIEW_ONLY.
```

## Safety Rules

- Never provide passwords, API keys, access tokens, or personal identifiers to Copilot Chat or place them in shell command arguments.
- Do not use `scripts/run-and-capture` with a command that exposes a secret; it stores the command arguments and output log as evidence.
- Add code comments only to explain non-obvious intent, constraints, or trade-offs. Add application logging only when its event, level, fields, and destination are explicitly approved.
- Keep protected reference and writable workspace directories separate when using `scripts/compare-workspaces`.
- Treat a passing validation command as evidence of the covered behavior only, not proof of complete correctness or production readiness.
