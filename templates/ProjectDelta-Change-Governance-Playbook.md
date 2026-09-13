# Project Delta Change Governance Playbook (Proposed Approach for Future Projects)

**Status:** Proposed — synthesized from the completed ProjectDelta evidence trail (`evidence/ProjectDelta/`) across the Enpea/FOG/ENPEA-BILL tracks.
**Purpose:** Capture the phase sequence, artifact taxonomy, and hard-won guardrails that emerged over the course of ProjectDelta, so a future project can start from a working process instead of re-deriving it.
**Relationship to other templates:** This document sits above the individual templates (`change-request.md`, `implementation-authorization.md`, `human-decision.md`, `pilot-measurements.md`, `change-evidence-packet.json`, `change-summary.md`, `InitialGroupedAction.md`, `ProjectDelta-Action-Inventory-Standard.md`) — it explains when and why each one gets produced, not what fields it contains.

## Why this exists

ProjectDelta's earliest work was not writing code — it was building an evidence-backed understanding of two legacy repositories (`OLT.Enpea` frontend, FOG backend) before proposing any change, then reusing that same discipline for every change that followed. That ordering — understand, then propose, then authorize, then execute, then validate, then decide — is the reusable part. This playbook makes that ordering explicit so it can be applied to a new project from day one rather than discovered partway through.

## The phase sequence

### Phase 0 — Repository readiness (read-only baseline)

Before any change work starts on a target repository, produce a `repository-readiness.md`: Git identity and HEAD (or an explicit `NOT_A_GIT_REPOSITORY` finding if the working copy isn't a clone), tech stack, architecture observations, a quality-control matrix, and a menu of hardening recommendations. This phase ends at `READY_FOR_HUMAN_BASELINE_REVIEW` — it proposes nothing and authorizes nothing.

**Lesson carried forward:** the readiness report's own suggested next step is not necessarily the actual next step. In ProjectDelta, the human declined the offered hardening recommendations and redirected the first real work to a different assignment entirely. Treat readiness output as input to a human decision, not as a queue.

### Phase 1 — Understand existing logic (disposition mapping)

Before proposing any change, produce a disposition map: classify every meaningful piece of the codebase against the target operating model as **Reuse / Strengthen / Adapt / Replace / Retire / Missing (build new)**, with path, symbol, current behavior, dependency, risk-if-unchanged, target capability, and a confidence/open-question column.

This phase is strictly read-only. State the non-goals explicitly every time: no edits, no builds, no tests, no package installs, and never treat current behavior as a requirement just because it exists.

### Phase 2 — Consequential-action inventory (trigger-to-boundary tracing)

Trace every inbound trigger (user action, event, webhook, job) to its state-changing or externally observable boundary. Use **two linked tables joined by a stable Action ID**, not one blended table:

- **Table A (action and consequence):** trigger, caller, path/symbol, affected subject or resource, classification, immediate consequence, counterpart ID.
- **Table B (controls and outcome evidence):** authentication, account/resource access, agency mandate and action-specific authority, validation, external instruction and destination, immediate transport result, confirmed business outcome, idempotency, reversibility, audit/correlation evidence, failure/escalation ownership, confidence/unresolved question.

**Do this from the start — don't repeat ProjectDelta's own rework.** ProjectDelta's first version of this inventory used a single blended `Actor/Authority` column, which made it easy to record "authenticated" and silently imply access, mandate, and completion were also established. That was caught and split into the two-table format above only after both repositories' inventories had already been built once. Start future projects directly with the two-table format.

**Vocabulary discipline:** use exactly four terms for anything not positively observed — `Absent` (inspected, doesn't exist), `Unknown` (inspected, can't determine), `Not applicable` (justify inline; never a default used to avoid answering), `Not inspected` (a coverage gap — list it explicitly, don't hide it by omission). Never use them interchangeably.

**Initiation and confirmation are separate actions.** Never let one row's "immediate transport result" (HTTP 200, ack, publish confirmation) stand in for "confirmed business outcome." If completion is actually established by a different code path, consumer, or callback, record that as its own linked Action ID via the Counterpart ID column — don't fold it into the initiating row.

**Never invent identifiers.** Counterpart IDs may only be assigned after evidence-backed reconciliation. Use `UNRESOLVED` while matching is pending and `NONE` only once investigation confirms there is no counterpart.

### Phase 3 — Cross-repository reconciliation

When more than one repository (or more than one independent pass of the same repository) produces an inventory, reconcile them against each other and against a fresh re-read of source before trusting either. Expect to find wrong edges (a call mapped to the wrong counterpart), overstated claims, and stale assumptions — this is normal, not a failure of Phase 2.

Pin every correction to evidence: record a SHA-256 hash of each source file cited so the reconciliation's claims are tied to exact bytes, not to memory of what the file said. Preserve the inventory's growth in stages (e.g., an initial supplement, then a scope-narrowed final pass) as superseded snapshots rather than editing in place.

### Phase 4 — Per-change lifecycle

Every bounded change, from here on, follows the same sequence:

1. **`change-request.md`** — intent, owning code path, authorized workspace, protected reference (Git HEAD or a hash manifest if no `.git` exists), authorized/candidate/reference-only files, required behavior, behavior that must not change, proposed validation commands, non-goals, rollback plan, and a traceable link back to the Phase 0–3 finding that motivated it. May leave explicit open questions for a human to answer. This is a **proposal**, not a grant.
2. **`implementation-authorization.md`** — a *separate*, human-completed permission slip. Names the exact authorized workspace, the exact writable files (literal paths, not "the module"), exact authorized commands, authorized logging (event name/level/fields), package-restore policy (default: no), prohibited operations, source-identity requirement, evidence directory, and an expiry condition (e.g., "voided if HEAD changes"). Starts `NOT_GRANTED`; only a human flips it to `GRANTED`. Use a numbered amendment (`-amendment-01.md`) when the *environment* — not the code — needs a narrowly-scoped fix (see Guardrail 6 below), never a scope-widening rewrite of the original.
3. **Implementation**, strictly inside what was granted.
4. **Evidence capture:** `source-identity-before.json` / `source-identity-after.json` (hash manifest of every authorized file; substitutes for a Git diff when the repo has no `.git`), `workspace-comparison.json` (catches drift *outside* the authorized manifest), and `validation/` logs for every authorized command actually run.
5. **`change-evidence-packet.json`** — the schema-validated, machine-checkable rollup: intent, non-goals, changed files/members, affected boundaries, design rationale, behavior preserved, executed validation (with exit codes and artifact references), unavailable validation (named, not hidden), privacy exclusions, uncertainties, rollback instructions, and next permitted action.
6. **`change-summary.md`** — the human-readable narrative of the same packet. Ends `HUMAN_REVIEW_ONLY`.
7. **`human-decision.md`** — the actual verdict: `ACCEPTED` / `ACCEPTED_WITH_CHANGES` / `NEEDS_MORE_EVIDENCE`, findings, required corrections, rationale, next permitted action. If something went procedurally wrong even though the content was safe, say so here explicitly as a process note (see Guardrail 8).
8. **`pilot-measurements.md`** (optional but recommended) — measures the *review experience*, not the code: time to review, reviewer confidence before/after, evidence completeness, unsupported-claim count, false-positive count. If the change never reached runnable validation, say plainly that no pilot was executed rather than fabricating one.

## Guardrails to carry forward unchanged

1. **Authentication ≠ authority ≠ completion.** Never let a positive answer in one control column imply a positive answer in another. This was the single largest structural correction ProjectDelta made to its own process — build it in from the start.
2. **Discovery is read-only by category, not by promise.** Phases 0–3 run zero builds, tests, or installs. State this as an explicit non-goal in every discovery artifact, not just as an assumed default.
3. **Prior authorization never carries forward.** A change request is not an authorization. An authorization for Phase 1 of a change does not extend to Phase 2. An authorization to create/switch a local branch does not extend to staging, committing, or pushing. Each action that touches shared state needs its own explicit grant.
4. **Supersede, never overwrite.** Any time a document, standard, or evidence file is corrected, preserve the prior version under `superseded/` with a dated filename instead of editing in place. This applies to templates, human-decisions, evidence packets, and validation results alike — even a blank or failed attempt is worth preserving.
5. **Fail closed over silent inference.** Don't invent a monetary value, an Action ID, a counterpart, or a completion status because the alternative is an unanswered cell. Mark it `Unknown`, `Absent`, or `UNRESOLVED` and move on.
6. **Environment failures are not scope failures.** If a sandbox, missing toolchain component, or write-permission issue blocks validation, authorize a narrowly-scoped fix for that specific problem (e.g., redirecting build output through an approved path) — don't use it as license to expand what code or behavior is in play. If it can't be fixed narrowly, record the blockage honestly (`NEEDS_MORE_EVIDENCE`) rather than skipping the check or fabricating a pass.
7. **Hash-based identity is an acceptable Git substitute.** When a target repository has no `.git` (a common state for extracted/legacy codebases), use SHA-256 manifests before and after as the protected reference and rollback basis. Don't treat the absence of Git as a reason to skip identity tracking.
8. **Procedural overreach gets flagged even when the outcome was harmless.** If an agent does something outside its authorization — even something that turns out safe (e.g., pushing already-reviewed content to a feature branch with no CI and no merge) — record it as a deviation in the human-decision, not as a non-event. Recommend the specific authorization-language fix (e.g., "any authorization intending to permit publish operations must name the branch/remote explicitly, plus a pre-push confirmation step") so the next project's templates close the gap.
9. **Missing governance artifacts get marked as gaps, not filled in by inference.** If a referenced architecture SSOT, authority matrix, or policy document doesn't actually exist in the repository, confirm that by direct search and mark every dependent question `NOT_INSPECTED` — don't reconstruct the missing document's intent from context.
10. **A live discovery finding can invalidate an earlier working assumption — let it.** If mid-project evidence (e.g., a plan document, a live 401 against a real service) contradicts an assumption an earlier change was built on, treat the new evidence as authoritative and update downstream change requests accordingly, rather than defending the original assumption.

## Validation checklist (apply what's relevant per change)

- Pre/post source-identity hashing (SHA-256) of every authorized file
- Workspace comparison outside the authorized manifest
- Build (clean compile, 0 warnings expected)
- Focused/unit tests (new behavior covered, no regression)
- Format verification (no uncommitted formatting drift)
- Secret scan of every proposed file (report rule/path/line only — never matched values)
- Schema/contract identity check where a shared payload contract exists
- Authorized-manifest check (exact file set matches what was granted, no extras/omissions)
- Git inspection (HEAD unchanged, no unexpected staged paths) where `.git` exists
- Health/smoke check against a loopback instance before claiming live-integration behavior
- Privacy/logging exclusion review (no PII, tokens, payloads, or raw exceptions in logs or evidence)
- Rollback instructions, stated even when "no rollback required" — tie back to the recorded pre-change hashes

Not every change will need every check. When a check can't run (environment defect, missing dependency), name it explicitly as unavailable validation rather than omitting it silently.

## Artifact quick reference

| Artifact | Phase | One-line purpose |
|---|---|---|
| `repository-readiness.md` | 0 | Read-only baseline of a target repo before any work starts |
| `assignment-1-repository-disposition-map.md` | 1 | Reuse/Strengthen/Adapt/Replace/Retire/Missing classification of existing logic |
| `assignment-2-consequential-action-inventory.md` (Table A) + `assignment-2-table-b-controls-and-outcomes.md` (Table B) | 2 | Trigger-to-boundary action trace, joined by stable Action ID |
| `reconciliation-review.md` / `reconciliation-master-map.md` + verification JSONs | 3 | Cross-checks inventories against each other and against hashed source |
| `change-request.md` | 4.1 | Proposed change: intent, scope, required/unchanged behavior, open questions |
| `implementation-authorization.md` (+ amendments) | 4.2 | Human-granted permission: exact files, commands, prohibitions, expiry |
| `source-identity-before.json` / `-after.json`, `workspace-comparison.json` | 4.4 | Hash-based before/after and drift evidence |
| `validation/*.log`, `*.result.json` | 4.4 | Captured output of every authorized validation command |
| `change-evidence-packet.json` | 4.5 | Machine-checkable rollup of the whole change |
| `change-summary.md` | 4.6 | Human-readable narrative of the packet |
| `human-decision.md` | 4.7 | The actual verdict and any process notes |
| `pilot-measurements.md` | 4.8 | Measures the review experience, not the code |

## Applying this to a new project

1. Copy this playbook and the referenced templates into the new project's `templates/` folder unchanged.
2. Run Phase 0 for every target repository before any other work.
3. Run Phases 1–2 in the two-table format from the start — do not reproduce the v1 single-column mistake.
4. If more than one repository is involved, budget explicit time for Phase 3 reconciliation; expect it to surface real corrections, not just confirm Phase 2.
5. For every subsequent change, follow the Phase 4 lifecycle in full, even for small changes — the evidence discipline is what makes review fast and trustworthy, not optional overhead to skip under time pressure.
