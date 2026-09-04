# Use the Harness in VS Code with Codex or Claude

This guide is for developers using VS Code with Codex or Claude. The harness guides a change workflow; it does not automatically authorize edits or replace human review.

## One-Time Workspace Setup

1. Keep this harness in its own local directory, outside the application repository. In VS Code, open the application folder, then select **File → Add Folder to Workspace** and add the harness directory. Save the resulting multi-root workspace if you want to reuse it. In this guide, `<harness-path>` means that separate local directory; replace it with its actual path in shell commands and agent prompts. Do not copy the harness into the application repository. When you record this path in a shared instruction file, prefer a relative path such as `../Agent-Change-Confidence-Harness` rather than a user-specific absolute path.

   ```sh
   ls "<harness-path>"
   ```

   Continue only when the output includes `prompts`, `templates`, `scripts`, and `schema`.
2. Create or update the application's root `AGENTS.md`. If the application already has that file, add the following harness rules without removing its existing project-specific instructions:

   Before pasting, replace every `<harness-path>` below with the local harness path relative to the application when possible (for example, `../Agent-Change-Confidence-Harness`). Do not leave the placeholder or a user-specific absolute path in a shared `AGENTS.md`.

   ```md
   # Agent Change Confidence Harness

   Use this workflow for a requested code change. The harness makes change scope and evidence reviewable; it does not grant permission to modify code, access services, or deploy.

   1. Before editing, obtain a completed change request with the intended behavior, exact authorized files, non-goals, privacy exclusions, validation command, and rollback plan. Follow `<harness-path>/prompts/01-discovery.md` for read-only discovery.
   2. Do not edit until a human has completed `<harness-path>/templates/implementation-authorization.md`. Treat only the listed workspace, files, permitted commands, and next action as authorized.
   3. Before and after an authorized change, use `<harness-path>/scripts/establish-source-identity` to hash each authorized file. Use `<harness-path>/scripts/compare-workspaces` against a protected reference when one is available.
   4. Implement only the approved scope. Follow `<harness-path>/prompts/02-implementation.md`; preserve stated behavior outside that scope. Add code comments only for non-obvious intent or constraints, and add application logging or telemetry only when its event, level, fields, and destination are authorized.
   5. Capture focused validation with `<harness-path>/scripts/run-and-capture`, then record the outcome in `<harness-path>/templates/change-evidence-packet.json` and validate it with `<harness-path>/scripts/validate-evidence`.
   6. Never include passwords, access tokens, API keys, personal identifiers, request payloads, raw exception contents, or other sensitive values in command arguments, logs, or evidence artifacts.
   7. Once the packet is complete, stop at `HUMAN_REVIEW_ONLY`. A reviewer or PM must make the next decision using the supplied summary and decision templates.

   Keep every request, authorization, log, and evidence artifact in `<harness-path>/evidence/`, outside the application workspace. Do not create an `evidence/` directory in the application repository. Do not change or overwrite prior evidence without preserving the superseded copy. Treat the harness's reusable prompts, templates, scripts, and schema as read-only; only its change-specific `evidence/` directory may be updated.
   ```
3. For Claude, create or update the application's root `CLAUDE.md` with the following line. If it already contains project-specific instructions, add this line rather than replacing them:

   ```md
   @AGENTS.md
   ```

4. Decide whether evidence is committed with the harness repository. The application repository does not contain harness evidence, so it does not need an `evidence/` ignore rule for this workflow. Create a project namespace and change-specific evidence directory under the harness after the human supplies the project name and change ID.
5. Verify the local prerequisites:

```sh
bash --version
jq --version
shasum --version
jsonschema --version
```

Run the setup verification prompt in a new chat. It returns the next permitted action, so the user does not need to infer the next step.

## Per-Change Workflow

1. Obtain the project name and assign a change ID. The human supplies both; an agent must not invent either. Use a filesystem-safe project name and the team's ticket or change ID.
2. Create the project and change evidence directory in the external harness, then copy the request template into it. Replace `<Project Name>` and `<Change ID>` before running these commands. Never create either directory in the application workspace or under the harness's `prompts/` directory:

   ```sh
   mkdir -p "<harness-path>/evidence/<Project Name>/<Change ID>"
   cp "<harness-path>/templates/change-request.md" \
     "<harness-path>/evidence/<Project Name>/<Change ID>/change-request.md"
   ```

3. Start the copied `change-request.md` file with what the human knows. It is normal for `<harness-path>/evidence/<Project Name>/<Change ID>/change-request.md` to be incomplete: fill in the intent, known constraints, and any decisions already made. Leave unknown fields blank rather than guessing. Each project/change folder has one canonical request file; do not create a second copy under another name. Do not treat this as implementation authorization.
4. Run the discovery prompt below. The agent must remain read-only with respect to the application workspace. It inspects the local codebase, identifies unanswered fields, and makes evidence-based recommendations. Use a Q&A session to narrow each decision: ask what the agent recommends, ask what to consider, then accept, reject, or revise its recommendation. The agent may update `change-request.md` only after the human agrees to the values being recorded. Save its generated discovery response as `<harness-path>/evidence/<Project Name>/<Change ID>/discovery.md` for human review. Do not copy or modify `<harness-path>/prompts/01-discovery.md`; it remains the reusable prompt.
5. When the request is complete, a human reviews the discovery record and prepares the separate implementation authorization described below. The authorization must name the writable workspace, allowed files, commands, prohibited operations, and next permitted action.
6. Run the implementation prompt below in a new edit-permitted session. The agent may edit only after the completed authorization is provided.
7. Review the generated packet, summary, validation logs, and uncertainties. Stop at `HUMAN_REVIEW_ONLY` until a reviewer or PM records the next decision.

## Recommended Chat Prompts

### Step 1: Verify One-Time Setup

Use this as the first prompt in a fresh project-agent chat, after the human has added the external harness folder to the VS Code workspace and configured the application's `AGENTS.md`:

```text
Use <harness-path>/prompts/00-setup-verification.md.
The application workspace is: <application-workspace-path>.
The local harness directory is: <harness-path>.
Remain read-only. Return the setup checkpoint followed by the templated next-step instructions.
```

### Optional Baseline: Map Application Entry Points

After the human has reviewed a completed repository-readiness report, use this read-only follow-on action to map the UI, API, service, queue, worker, and scheduled-job paths that could affect a future change:

```text
Use <harness-path>/prompts/pre-change-application-entry-points.md.

The target repository is: <application-workspace-path>.
The completed readiness report is:
<harness-path>/evidence/<Project Name>/readiness/<Assessment ID>/repository-readiness.md.
Return the report for a human to save at:
<harness-path>/evidence/<Project Name>/readiness/<Assessment ID>/application-entry-points.md.

Do not modify the target repository, start services, access external systems, submit requests or messages, or create the report file unless I explicitly authorize that external write.
```

The report is contextual evidence only. If its current repository identity or `HEAD` does not match the readiness report, refresh readiness before using the map for change planning.

Use a prompt like this for discovery:

```text
Use <harness-path>/prompts/01-discovery.md.
The current change request is at: <change-request-path>.
Remain read-only. Return the discovery record for a human to save as <discovery-record-path>.
```

If the human explicitly asks the agent to initialize the external evidence directory, use this setup prompt before discovery:

```text
Create only this external harness evidence directory and copy only the change-request template into it:
<harness-path>/evidence/<Project Name>/<Change ID>/

Do not create or modify files in the application workspace. Do not modify reusable files under <harness-path>/prompts/, templates/, scripts/, or schema/.
```

Replace the placeholders with the actual files for this change. The request and discovery-record paths must be under `<harness-path>/evidence/<Project Name>/<Change ID>/`, never under the application workspace. The canonical request is `change-request.md`; `discovery.md` is generated only after discovery completes. Do not copy either reusable prompt into the evidence directory.

### Use Discovery as a Decision Conversation

You do not need every field in `change-request.md` before starting. Begin with the change intent and the facts you know, then let the agent use read-only repository evidence to surface the remaining decisions. It should give a concrete recommendation for each open or ambiguous field, explain the relevant evidence and trade-offs, and ask for your decision. Recommendations are proposals, not approval.

Useful follow-up questions include:

- `What do you recommend for question 5, and why?`
- `What should I consider when making this decision?`
- `What repository evidence supports that recommendation?`
- `What is the narrowest safe option?`
- `Revise the recommendation to account for <constraint>.`

When you are ready, clearly accept, reject, or revise the recommendations by question number. For example: `Accept 5 and 9; revise 7 to use source-identity hashes; what do you recommend for 12?` If an acknowledgement could be ambiguous (for example, "all three" after four recommendations), clarify the question numbers before asking the agent to record anything. Once you agree to the final set, tell the agent to record those agreed values in `change-request.md`. It must change only the request file at this stage; source code, tests, configuration, and authorization files remain untouched.

### Prepare Human Implementation Authorization

After discovery confirms that the request is complete, the human creates a new, change-specific authorization file by copying the template. This file is not pre-existing and must not be created by the implementation agent. Replace `<Change ID>` with the same unique value used for the request:

```sh
cp "<harness-path>/templates/implementation-authorization.md" \
  "<harness-path>/evidence/<Project Name>/<Change ID>/implementation-authorization.md"
```

The human—not the agent—completes and approves this file. Carry forward only the agreed scope from the request and discovery record: the writable workspace, exact source and test files, permitted source-identity and validation commands, prohibited operations, privacy exclusions, rollback, and next permitted action. Reference files remain read-only. Do not start the implementation prompt while this file is missing, incomplete, or unapproved; the implementation agent must stop in that case. Do not authorize implementation until every value is explicit and the human has approved it.

After approval, start a new edit-permitted agent session with this prompt. Replace the placeholder with the actual authorization artifact path:

Use this only after human authorization:

```text
Use <harness-path>/prompts/02-implementation.md.
The completed implementation authorization with human approval is at: <implementation-authorization-path>.
Work only within that authorization. Capture the required evidence and stop at HUMAN_REVIEW_ONLY.
```

## Safety Rules

- Never provide passwords, API keys, access tokens, or personal identifiers to agent chat or place them in shell command arguments.
- Do not use `<harness-path>/scripts/run-and-capture` with a command that exposes a secret; it stores the command arguments and output log as evidence.
- Add code comments only to explain non-obvious intent, constraints, or trade-offs. Add application logging only when its event, level, fields, and destination are explicitly approved.
- Keep protected reference and writable workspace directories separate when using `<harness-path>/scripts/compare-workspaces`.
- Treat a passing validation command as evidence of the covered behavior only, not proof of complete correctness or production readiness.
