# Use the Harness in VS Code with Codex or Claude

This guide is for developers using VS Code with Codex or Claude. The harness guides a change workflow; it does not automatically authorize edits or replace human review.

## One-Time Workspace Setup

1. Copy this package into the repository as `tools/Agent-Change-Confidence-Harness`:

   ```sh
   mkdir -p tools
   cp -R "/path/to/Agent-Change-Confidence-Harness" \
     "tools/Agent-Change-Confidence-Harness"
   ls tools/Agent-Change-Confidence-Harness
   ```

   Continue only when the output includes `prompts`, `templates`, `scripts`, and `schema`.
2. Create or update the application's root `AGENTS.md`. If the application already has that file, add the following harness rules without removing its existing project-specific instructions:

   ```md
   # Agent Change Confidence Harness

   Use this workflow for a requested code change. The harness makes change scope and evidence reviewable; it does not grant permission to modify code, access services, or deploy.

   1. Before editing, obtain a completed change request with the intended behavior, exact authorized files, non-goals, privacy exclusions, validation command, and rollback plan. Follow `tools/Agent-Change-Confidence-Harness/prompts/01-discovery.md` for read-only discovery.
   2. Do not edit until a human has completed `tools/Agent-Change-Confidence-Harness/templates/implementation-authorization.md`. Treat only the listed workspace, files, permitted commands, and next action as authorized.
   3. Before and after an authorized change, use `tools/Agent-Change-Confidence-Harness/scripts/establish-source-identity` to hash each authorized file. Use `tools/Agent-Change-Confidence-Harness/scripts/compare-workspaces` against a protected reference when one is available.
   4. Implement only the approved scope. Follow `tools/Agent-Change-Confidence-Harness/prompts/02-implementation.md`; preserve stated behavior outside that scope. Add code comments only for non-obvious intent or constraints, and add application logging or telemetry only when its event, level, fields, and destination are authorized.
   5. Capture focused validation with `tools/Agent-Change-Confidence-Harness/scripts/run-and-capture`, then record the outcome in `tools/Agent-Change-Confidence-Harness/templates/change-evidence-packet.json` and validate it with `tools/Agent-Change-Confidence-Harness/scripts/validate-evidence`.
   6. Never include passwords, access tokens, API keys, personal identifiers, request payloads, raw exception contents, or other sensitive values in command arguments, logs, or evidence artifacts.
   7. Once the packet is complete, stop at `HUMAN_REVIEW_ONLY`. A reviewer or PM must make the next decision using the supplied summary and decision templates.

   Keep evidence output outside application source and do not change or overwrite prior evidence without preserving the superseded copy.
   ```
3. For Claude, create or update the application's root `CLAUDE.md` with the following line. If it already contains project-specific instructions, add this line rather than replacing them:

   ```md
   @AGENTS.md
   ```

4. Decide whether evidence is committed. If it is not, add `evidence/` to the application's ignore rules now; create the change-specific evidence directory after the change request supplies its ID.
5. Verify the local prerequisites:

```sh
bash --version
jq --version
shasum --version
jsonschema --version
```

Run the setup verification prompt in a new chat. It returns the next permitted action, so the user does not need to infer the next step.

## Per-Change Workflow

1. Obtain or assign a change ID. The human completing the request supplies it; an agent must not invent it. Use the team's ticket or change ID.
2. Create the change evidence directory and copy the request template into it. Replace `<Change ID>` before running these commands:

   ```sh
   mkdir -p "evidence/<Change ID>"
   cp "tools/Agent-Change-Confidence-Harness/templates/change-request.md" \
     "evidence/<Change ID>/first-change.md"
   ```

3. Start the copied request file with what the human knows. It is normal for `evidence/<Change ID>/first-change.md` to be incomplete: fill in the intent, known constraints, and any decisions already made. Leave unknown fields blank rather than guessing. Do not treat this as implementation authorization.
4. Run the discovery prompt below. The agent must remain read-only. It inspects the local codebase, identifies unanswered fields, and makes evidence-based recommendations. Use a Q&A session to narrow each decision: ask what the agent recommends, ask what to consider, then accept, reject, or revise its recommendation. The agent may update `first-change.md` only after the human agrees to the values being recorded. Save its discovery response as `evidence/<Change ID>/discovery.md` for human review.
5. When the request is complete, a human reviews the discovery record, copies `tools/Agent-Change-Confidence-Harness/templates/implementation-authorization.md` to `evidence/<Change ID>/implementation-authorization.md`, and completes it. The authorization must name the writable workspace, allowed files, commands, prohibited operations, and next permitted action.
6. Run the implementation prompt below in a new edit-permitted session. The agent may edit only after the completed authorization is provided.
7. Review the generated packet, summary, validation logs, and uncertainties. Stop at `HUMAN_REVIEW_ONLY` until a reviewer or PM records the next decision.

## Recommended Chat Prompts

Use this after the one-time setup:

```text
Use tools/Agent-Change-Confidence-Harness/prompts/00-setup-verification.md.
Remain read-only and return the required setup checkpoint exactly.
```

Use a prompt like this for discovery:

```text
Use tools/Agent-Change-Confidence-Harness/prompts/01-discovery.md.
The current change request is at: <change-request-path>.
Remain read-only. Return the discovery record for a human to save as <discovery-record-path>.
```

Replace the placeholders with the actual files for this change. For example, the request might be `evidence/<Change ID>/first-change.md`; that example is not a required filename.

### Use Discovery as a Decision Conversation

You do not need every field in `first-change.md` before starting. Begin with the change intent and the facts you know, then let the agent use read-only repository evidence to surface the remaining decisions. It should give a concrete recommendation for each open or ambiguous field, explain the relevant evidence and trade-offs, and ask for your decision. Recommendations are proposals, not approval.

Useful follow-up questions include:

- `What do you recommend for question 5, and why?`
- `What should I consider when making this decision?`
- `What repository evidence supports that recommendation?`
- `What is the narrowest safe option?`
- `Revise the recommendation to account for <constraint>.`

When you are ready, clearly accept, reject, or revise the recommendations by question number. For example: `Accept 5 and 9; revise 7 to use source-identity hashes; what do you recommend for 12?` If an acknowledgement could be ambiguous (for example, "all three" after four recommendations), clarify the question numbers before asking the agent to record anything. Once you agree to the final set, tell the agent to record those agreed values in `first-change.md`. It must change only the request file at this stage; source code, tests, configuration, and authorization files remain untouched.

Use this only after human authorization:

```text
Use tools/Agent-Change-Confidence-Harness/prompts/02-implementation.md.
The completed implementation authorization with human approval is at: <implementation-authorization-path>.
Work only within that authorization. Capture the required evidence and stop at HUMAN_REVIEW_ONLY.
```

## Safety Rules

- Never provide passwords, API keys, access tokens, or personal identifiers to agent chat or place them in shell command arguments.
- Do not use `tools/Agent-Change-Confidence-Harness/scripts/run-and-capture` with a command that exposes a secret; it stores the command arguments and output log as evidence.
- Add code comments only to explain non-obvious intent, constraints, or trade-offs. Add application logging only when its event, level, fields, and destination are explicitly approved.
- Keep protected reference and writable workspace directories separate when using `tools/Agent-Change-Confidence-Harness/scripts/compare-workspaces`.
- Treat a passing validation command as evidence of the covered behavior only, not proof of complete correctness or production readiness.
