# Feature: Ideate Skill

> [View in Synchestra Hub](https://hub.synchestra.io/project/features?id=spec-studio@synchestra-io@github.com&path=spec%2Ffeatures%2Fskills%2Fideate) — graph, discussions, approvals

**Status:** In Progress

## Summary

The `spec-studio:ideate` skill refines raw, vague concepts into lint-clean SpecScore Idea artifacts through a three-phase dialogue (Understand & Expand, Evaluate & Converge, Crystallize). Its output (`spec/ideas/<slug>.md`) is the gating input for `spec-studio:specify` and the only sanctioned way to produce a SpecScore Idea inside Spec Studio. Implementation lives at [`skills/ideate/`](../../../../skills/ideate/).

## Problem

Vague intent silently corrupts every downstream artifact. When a developer tells an AI agent "build me X" without first articulating the problem, alternatives, and explicit non-goals, the agent fills in the gaps stochastically — producing features that solve the wrong problem, miss the obvious "Not Doing" boundary, or inherit unstated assumptions as if they were specifications.

`ideate` exists to pay the ideation cost up front and capture it in a typed artifact, instead of paying it implicitly across every later phase of the lifecycle. The Feature defines exactly what that capture must look like, so the artifact is machine-verifiable and downstream skills can rely on it.

## Behavior

### Invocation and skip conditions

The skill is invoked when a concept is vague, multiple directions are open, or the user wants to stress-test an idea before specifying it.

#### REQ: invocation-triggers

The skill MUST respond to the triggers `ideate`, `/ideate`, "refine this idea", and "stress-test this". It MAY respond to additional natural-language phrasings of the same intent.

#### REQ: skip-when-clear-intent

The skill MUST NOT proceed when the user already has a clear, high-conviction feature to specify. In that case it MUST hand off to `spec-studio:specify` and explain why. The decision criterion is whether the user can articulate the problem, the recommended direction, and an explicit Not-Doing boundary in one short statement.

### Hard gate

The skill is gated downstream — once invoked, it must produce a complete, lint-clean, user-approved Idea before any other Spec Studio skill can run.

#### REQ: hard-gate

The skill MUST NOT invoke `spec-studio:specify`, `writing-plans`, or any implementation skill until ALL THREE conditions hold:

1. An Idea artifact exists at `spec/ideas/<slug>.md`.
2. `specscore lint spec/ideas/<slug>.md` exits zero.
3. The user has explicitly approved the Recommended Direction.

A skill invocation that bypasses any condition is a contract violation.

### Artifact location

Idea artifacts have one canonical location.

#### REQ: artifact-path

The skill MUST write Idea artifacts to `spec/ideas/<slug>.md` relative to the project root. The `<slug>` MUST be lowercase, hyphen-separated, URL-safe, and stable for the artifact's lifetime.

#### REQ: no-docs-path

The skill MUST NOT write Idea artifacts to `docs/ideas/`, `notes/`, or any path other than `spec/ideas/`. `docs/` is reserved for prose documentation; SpecScore artifacts live under `spec/`.

### Three-phase dialogue

The skill runs a fixed three-phase structure: divergent exploration, convergent evaluation, and crystallization into the artifact.

#### REQ: phase-1-divergent

In Phase 1 the skill MUST: (a) restate the user's input as a single "How Might We…" sentence, (b) ask 3–5 sharpening questions in batched form covering at minimum *who is this for* and *what does success look like*, and (c) generate 5–8 directional variations using lenses such as inversion, constraint removal, audience shift, combination, simplification, 10x, or expert lens.

#### REQ: phase-2-convergent

In Phase 2 the skill MUST: (a) cluster the variations into 2–3 meaningfully different directions, (b) stress-test each direction on user value, feasibility, and differentiation, and (c) surface hidden assumptions tagged Must-be-true / Should-be-true / Might-be-true.

#### REQ: phase-3-crystallize

In Phase 3 the skill MUST produce the Idea artifact at the canonical path. The artifact's content MUST conform to the Idea schema documented in the skill manifest, including all required sections.

### CLI vs fallback path

Crystallization prefers a stable CLI contract over ad-hoc file writes.

#### REQ: cli-preferred

When the `specscore` CLI is on PATH, the skill MUST use `specscore new idea <slug>` to scaffold the artifact, passing every field it already has via the documented flags (`--title`, `--owner`, `--hmw`, `--context`, `--recommended-direction`, `--mvp`, `--not-doing`).

#### REQ: cli-flag-discipline

The skill MUST NOT invent CLI flags. If a needed input has no flag, the skill MUST fill that section via `Edit` after scaffolding.

#### REQ: fallback-direct-write

When the `specscore` CLI is NOT on PATH, the skill MUST fall back to a direct file write using the same authoritative schema. The fallback artifact MUST be byte-equivalent (modulo whitespace) to what the CLI would have produced.

### Required artifact content

Some sections are non-negotiable.

#### REQ: not-doing-required

The Idea artifact's `Not Doing (and Why)` section MUST be non-empty. This corresponds to lint rule `I-002`. An Idea without explicit exclusions is rejected.

#### REQ: assumption-tiers

The Idea artifact MUST include at least one Must-be-true assumption in the `Key Assumptions to Validate` table. This corresponds to lint rule `I-003`.

### Lint and self-review

Every artifact passes through both machine validation and a deliberate human-style review.

#### REQ: lint-pass

After writing or editing the artifact, the skill MUST run `specscore lint spec/ideas/<slug>.md` and confirm a zero exit code before proceeding to user review.

#### REQ: inline-self-review

Before requesting user review, the skill MUST scan the artifact for: (a) unresolved placeholders (`TBD`, `TODO`, `???`, `FIXME`), (b) internal contradictions (Recommended Direction vs. MVP Scope, assumptions vs. Not-Doing), (c) hidden multi-Idea scope, and (d) requirements interpretable two ways. Findings MUST be fixed inline.

### User review and approval

The user — not the skill — owns the approval decision.

#### REQ: user-approval-required

The skill MUST present the lint-clean artifact to the user with an explicit request to approve the Recommended Direction. The skill MUST NOT proceed to status transition or event emission without that approval.

#### REQ: status-transition-on-approval

On user approval, the skill MUST update the artifact's front-matter `status` from `Draft` to `Approved` and re-run lint to confirm the transition is still valid.

### Event emission

The skill participates in the Synchestra event vocabulary.

#### REQ: event-drafted

The skill MUST emit `idea.drafted` on first successful write of the artifact (after lint passes for the first time).

#### REQ: event-approved

The skill MUST emit `idea.approved` after the user approves the Recommended Direction and the status transition completes.

### Promotion boundary

Promotion to a Feature is the responsibility of `spec-studio:specify` and Synchestra tooling, not of `ideate`.

#### REQ: no-manual-promotes-to

The skill MUST NOT manually edit the `promotes_to` front-matter field. That field is managed by Synchestra in response to a Feature declaring its `Source Ideas`.

#### REQ: promotion-out-of-scope

The skill MUST NOT scaffold, write, or modify SpecScore Features. When a user requests promotion immediately after approval, the skill MUST hand off to `spec-studio:specify`.

### Tone

#### REQ: honest-pushback

The skill MUST NOT yes-machine weak ideas. When a direction has clear problems, the skill MUST say so with specificity and propose the alternative. The acceptance bar is honest disagreement, not performative agreement.

## Interaction with Other Features

| Feature | Interaction |
|---|---|
| [Specify Skill](../specify/README.md) | `ideate` is the upstream gate of `specify`. `specify` consumes the approved Idea via `Source Ideas` linkage; `ideate` never invokes `specify` itself — the user does. |
| [SpecScore Idea](https://github.com/synchestra-io/specscore/blob/main/spec/features/idea/README.md) | The schema, lint rules, and lifecycle of the produced artifact are owned by SpecScore's Idea feature. `ideate` is a producer, not a definer of that schema. |
| Synchestra Events | Emits `idea.drafted` and `idea.approved`. Consumers — including `specify` and Hub — observe these to advance their own state. |
| `specscore new idea` CLI | Preferred crystallization path. The skill probes for the CLI once per invocation and falls back to direct write only when absent. |

## Acceptance Criteria

### AC: hard-gate-enforced

**Requirements:** ideate#req:hard-gate, ideate#req:lint-pass, ideate#req:user-approval-required

The skill cannot invoke `specify`, `writing-plans`, or any implementation skill until the artifact exists at `spec/ideas/<slug>.md`, `specscore lint` passes, and the user has approved the Recommended Direction. Any invocation that bypasses any of these conditions is rejected.

### AC: artifact-conformance

**Requirements:** ideate#req:artifact-path, ideate#req:no-docs-path, ideate#req:phase-3-crystallize, ideate#req:not-doing-required, ideate#req:assumption-tiers

Every produced artifact lives at the canonical path `spec/ideas/<slug>.md`, conforms to the Idea schema, has a non-empty `Not Doing (and Why)` section, and lists at least one Must-be-true assumption. Artifacts written elsewhere or missing required sections are rejected by `specscore lint`.

### AC: phase-discipline

**Requirements:** ideate#req:phase-1-divergent, ideate#req:phase-2-convergent

The skill executes the three-phase dialogue in order. Phase 1 produces a HMW restatement, sharpening questions covering at minimum *who* and *success*, and 5–8 variations. Phase 2 produces 2–3 stress-tested directions with assumptions tiered into Must / Should / Might.

### AC: cli-vs-fallback

**Requirements:** ideate#req:cli-preferred, ideate#req:cli-flag-discipline, ideate#req:fallback-direct-write

The skill probes for the `specscore` CLI once per invocation. When present, scaffolding goes through `specscore new idea <slug>` with only documented flags. When absent, the skill writes the artifact directly using the authoritative schema, producing byte-equivalent output (modulo whitespace).

### AC: lifecycle-events

**Requirements:** ideate#req:event-drafted, ideate#req:event-approved, ideate#req:status-transition-on-approval

`idea.drafted` is emitted on first lint-clean write. On user approval, the front-matter `status` transitions Draft → Approved, lint is re-run, and `idea.approved` is emitted. Skipping or reordering these steps is a contract violation.

### AC: skip-condition-respected

**Requirements:** ideate#req:skip-when-clear-intent

When the user can articulate the problem, recommended direction, and Not-Doing boundary up front, the skill hands off to `spec-studio:specify` without running the three-phase dialogue. The hand-off is explicit, not silent.

### AC: promotion-boundary-held

**Requirements:** ideate#req:no-manual-promotes-to, ideate#req:promotion-out-of-scope

The skill never edits `promotes_to`, never scaffolds a Feature, and never modifies an existing Feature. Promotion requests are routed to `spec-studio:specify`.

## Outstanding Questions

- Should "explicit user approval" require a specific phrase (e.g., "approve") for unambiguous detection, or is any positive signal sufficient?
- When the user requests changes after lint has already passed, should the skill keep the same `idea.drafted` event or emit a new one?
- How should the skill handle a project with no `spec/ideas/` directory yet — auto-create, or refuse and require explicit project initialization?
- What is the precise contract for "byte-equivalent (modulo whitespace)" between CLI and fallback paths — should this be enforced by a comparison test, or is it a design intent?
- Should `idea.drafted` be emitted on every lint-clean re-write during iteration, or only on the first one?
- Does the skill need a recovery mode when `specscore lint` fails after an `Edit` — auto-fix-and-retry, or always surface to user?

---
*This document follows the https://specscore.md/feature-specification*
