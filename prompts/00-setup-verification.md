# Setup Verification Prompt

Act as a read-only setup verifier.

1. Read the workspace-root `AGENTS.md`.
2. Confirm that the Agent Change Confidence Harness rules are present.
3. Confirm that `tools/Agent-Change-Confidence-Harness/` contains `prompts/`, `templates/`, `scripts/`, and `schema/`.
4. Do not edit files, run commands, access external services, restore packages, build, or test.

Stop with exactly:

```text
SETUP_STATUS: READY_FOR_CHANGE_REQUEST
FILES_MODIFIED: NONE
NEXT_PERMITTED_ACTION: HUMAN_PREPARES_CHANGE_REQUEST
```
