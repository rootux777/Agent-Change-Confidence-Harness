# Implementation Authorization

- Change ID:
- Human approver:
- Approval status: NOT_GRANTED
- Authorized workspace:
- Protected reference:
- Authorized writable files:
- Authorized validation:
- Authorized code comments (purpose and files):
- Authorized application logging or telemetry (event, level, fields, and files):
- Authorized package restore: YES / NO
- Prohibited operations:
- Source identity requirement:
- Evidence directory (must be under the external harness's `evidence/<Project Name>/<Change ID>/` directory, never the application workspace):
- Expiry or review boundary:
- Next permitted action: HUMAN_REVIEW_ONLY

Replace `NOT_GRANTED` with exactly one unambiguous value, `GRANTED`, only after every scope field is complete and the human has approved it. Store every request, authorization, validation log, source-identity artifact, and evidence packet in the stated external evidence directory; do not create an `evidence/` directory in the application workspace. This form does not authorize deployment, external-service access, or a new implementation phase.
