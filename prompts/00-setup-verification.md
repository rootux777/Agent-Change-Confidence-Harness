# Setup Verification Prompt

Act as a read-only setup verifier.

1. Read the workspace-root `AGENTS.md`.
2. Confirm that the Agent Change Confidence Harness rules are present.
3. Confirm that the human-supplied local harness directory contains `prompts/`, `templates/`, `scripts/`, and `schema/`.
4. Do not edit files, run commands, access external services, restore packages, build, or test.

Return the checkpoint followed by the next-step instructions. Use the human-supplied harness path in the instructions and retain `<Project Name>` and `<Change ID>` as placeholders:

```text
SETUP_STATUS: READY_FOR_CHANGE_REQUEST
FILES_MODIFIED: NONE
NEXT_PERMITTED_ACTION: HUMAN_PREPARES_CHANGE_REQUEST

NEXT_STEP_INSTRUCTIONS:
1. Human supplies a filesystem-safe project name and a change ID.
2. Create <harness-path>/evidence/<Project Name>/<Change ID>/ outside the application workspace.
3. Copy <harness-path>/templates/change-request.md to <harness-path>/evidence/<Project Name>/<Change ID>/first-change.md.
4. Complete the known request fields, then start read-only discovery with <harness-path>/prompts/01-discovery.md.
```
