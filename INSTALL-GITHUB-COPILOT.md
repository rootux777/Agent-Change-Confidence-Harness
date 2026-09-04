# Install With GitHub Copilot

## Installation And Usability Test

### 1. Keep the harness beside the project

Clone or download the harness into its own local directory, outside the application repository. For example:

```yaml
git clone --depth 1 \
  https://github.com/rootux777/Agent-Change-Confidence-Harness.git \
  "/path/to/Agent-Change-Confidence-Harness"
```

If you already cloned it there, update it instead:

```
git -C "/path/to/Agent-Change-Confidence-Harness" pull --ff-only
```

Open the application in VS Code. Then select **File → Add Folder to Workspace**, add the local `Agent-Change-Confidence-Harness` folder, and save the multi-root workspace. The harness remains separate from the application: its reusable prompts, templates, scripts, and schema are read-only, while its `evidence/` directory holds copied templates and change evidence.

You should now see both folders in the VS Code workspace, for example:

```
your-application/
Agent-Change-Confidence-Harness/
```

Reference the harness with its local path, or with the workspace folder name when the agent can resolve it:

```
Agent-Change-Confidence-Harness/prompts/01-discovery.md
```

### 2. Start a Copilot Agent session

Open Copilot Chat in Agent mode and paste this:

```yaml
We are conducting an installation and usability test of the
Agent Change Confidence Harness.

Do not modify any files.
Do not restore packages, build, run tests, access external services,
or execute the harness scripts.

First, confirm that you can read these files:

- Agent-Change-Confidence-Harness/README.md
- Agent-Change-Confidence-Harness/INSTALL-GITHUB-COPILOT.md
- Agent-Change-Confidence-Harness/prompts/01-discovery.md
- Agent-Change-Confidence-Harness/templates/change-request.md
- Agent-Change-Confidence-Harness/templates/implementation-authorization.md

Then:

1. Summarize the harness workflow in your own words.
2. Explain the difference between a change request, discovery,
   implementation authorization, and human review.
3. Identify the information still needed before a read-only discovery
   can begin.
4. Do not begin implementation.
5. Do not create or edit the change-request template yet.
6. Stop with exactly:

CHECKPOINT_STATUS: READY_FOR_CHANGE_REQUEST_INPUT
FILES_MODIFIED: NONE
NEXT_PERMITTED_ACTION: HUMAN_INPUT_ONLY
```

## Keep As A Local Tool

Keep this package outside application source and keep evidence output separate from application files. Do not copy the harness into the application repository, publish it, or install it as a runtime dependency. Add its downloaded or cloned directory to the VS Code multi-root workspace instead.

**Verified in V0.1:** the package scripts use local shell commands, accept paths containing spaces when quoted, and do not require network access. The example packet validates with the `jsonschema` command available in the validation environment.

**Version-dependent guidance:** GitHub Copilot instruction discovery and repository customization locations can vary by editor, extension version, and team policy. Place prompts where your approved Copilot workflow can reference them; verify discovery behavior in the current editor before relying on it.

## Supply A Partial Change Request

Start with `templates/change-request.md` and fill in the facts you already know. It is acceptable and expected to leave owning paths, exact files, event names, decision boundaries, validation commands, or other fields unresolved.

Submit the partial request with `prompts/01-discovery.md`. The agent reads the request, inspects relevant local code and nearby tests without editing or running validation, identifies each missing or ambiguous value, and recommends concrete answers when repository evidence supports them. Review every recommendation with the human. The agent should ask follow-up questions and repeat until the human accepts or revises every required field.

After the human agrees to the complete request, the agent may write the agreed values into the named request file. The agent must not silently infer authorization or begin implementation. Complete `templates/implementation-authorization.md` separately before any source edit.

Example chat instruction:

```text
Read <harness-path>/evidence/<project-name>/<change-id>/change-request.md and <harness-path>/prompts/01-discovery.md.
Treat the request as potentially incomplete. Inspect local code and tests read-only,
recommend values for missing fields, and ask me to accept or revise them. Do not edit
the request file until I agree to every recommendation. Do not implement anything.
```

Verify prerequisites before use:

```sh
bash --version
jq --version
shasum --version
jsonschema --version
```

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
