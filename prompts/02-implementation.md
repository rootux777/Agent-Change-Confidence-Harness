# Implementation Prompt

Implement one bounded change only after explicit human authorization.

Before editing:

- Confirm the authorized workspace and protected reference are separate.
- Hash every authorized file before implementation.
- Search for identifier conflicts.
- Run only the authorized baseline validation.

During implementation:

- Modify only the authorized files.
- Preserve stated behavior and boundaries.
- Use existing repository patterns.
- Add comments only when they explain non-obvious intent, constraints, or a deliberate trade-off; do not restate the code.
- Keep structured logging fields privacy-conscious and explicit.
- Add application logging or telemetry only when the authorization names its event, level, fields, and destination. Reuse the repository's logger and conventions.
- Do not add payload, identifier, request serialization, or raw exception fields unless explicitly authorized.

After editing:

- Run the authorized focused validation.
- Capture the literal command, working directory, timestamps, exit code, and artifact references.
- Build only when authorized.
- Compare the protected reference and working copy with an explicit exclusion list.
- Hash authorized files after implementation.
- Produce the evidence packet and stop at `HUMAN_REVIEW_ONLY`.
