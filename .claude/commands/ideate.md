---
description: Refine a raw idea into a lint-clean SpecScore Idea artifact through structured divergent and convergent thinking
---

Invoke the `specscore:ideate` skill.

The skill runs a three-phase process — **Understand & Expand** (divergent), **Evaluate & Converge**, **Crystallize as a SpecScore Idea** — and gates on:

1. A lint-clean artifact at `spec/ideas/<slug>.md`.
2. `specscore lint` passes.
3. The user has explicitly approved the Recommended Direction.

If the user passed a raw idea as arguments (`$ARGUMENTS`), treat that as the seed for Phase 1. Otherwise, ask what they'd like to ideate on before starting.

Do not invoke `specscore:design`, `writing-plans`, or any implementation skill until the hard gate passes.
