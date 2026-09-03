# Install With GitHub Copilot

## Installation And Usability Test

### 1. Add the harness to the project

In the IDE terminal:

```yaml
cd "/YOUR PROJECT HERE"

mkdir -p tools

git clone --depth 1 \
  https://github.com/rootux777/Agent-Change-Confidence-Harness.git \
  tools/Agent-Change-Confidence-Harness
```

If you already cloned it there, update it instead:

```
git -C tools/Agent-Change-Confidence-Harness pull --ff-only
```

You should now see:

```
OLT.CarePortal (1)/
├── OLT.CarePortal.Core/
└── tools/
    └── Agent-Change-Confidence-Harness/
        ├── README.md
        ├── prompts/
        ├── templates/
        ├── scripts/
        └── schema/
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

- tools/Agent-Change-Confidence-Harness/README.md
- tools/Agent-Change-Confidence-Harness/INSTALL-GITHUB-COPILOT.md
- tools/Agent-Change-Confidence-Harness/prompts/01-discovery.md
- tools/Agent-Change-Confidence-Harness/templates/change-request.md
- tools/Agent-Change-Confidence-Harness/templates/implementation-authorization.md

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
