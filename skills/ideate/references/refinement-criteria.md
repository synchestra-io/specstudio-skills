# Refinement & Evaluation Criteria

Adapted from `addyosmani/agent-skills/skills/idea-refine/refinement-criteria.md`. Use during Phase 2 to stress-test idea directions.

## Core Evaluation Dimensions

### 1. User Value

The most important dimension. If value isn't clear, nothing else matters.

**Painkiller vs. Vitamin:**
- **Painkiller** — acute, frequent problem. Users actively seek this out; they'll switch. Signs: people describe the problem with emotion, built workarounds, will pay.
- **Vitamin** — nice to have. Signs: "that's cool," then no behavior change.

**Questions:**
- Can you name 3 specific people with this problem right now?
- What are they doing today instead? (The current workaround is the real competitor.)
- Would they switch? What would make them?
- How often? (Daily > monthly.)
- Pull (users ask for this) or push (you think they should want this)?

**Red flags:**
- "Everyone could use this" — can't name a user means unclear value.
- "Like X but better" — marginal improvements rarely drive adoption.
- Real-but-rare — high intensity, low frequency rarely justifies a product.

### 2. Feasibility

Can you actually build this? Not just technically, but practically.

**Technical:** Does the core tech work reliably? Known-hard or novel problem? Third-party dependencies? Minimum stack size?

**Resource:** Minimum team/effort for MVP? Specialized expertise? Regulatory/legal?

**Time-to-value:** Days/weeks or months to first user? Critical path?

**Red flags:**
- "We just need to solve [very hard research problem] first."
- Multiple dependencies that all need to work simultaneously.
- MVP still requires months.

### 3. Differentiation

What makes this genuinely *different* — not just better?

**Questions:**
- If a user described this to a friend, what would they say?
- What's the one thing this does that nothing else does?
- Is the differentiation durable? Can a competitor copy it in a week?
- Does the user care about the difference, or just the builder?

**Types (strongest → weakest):**
1. New capability (previously impossible)
2. 10x improvement on a key dimension
3. New audience (existing capability, excluded users)
4. New context (works where others fail)
5. Better UX (same capability, simpler)
6. Cheaper (easily competed away)

**Red flags:**
- Differentiation is technology, not experience.
- "Faster/cheaper/prettier" without a structural reason.
- The differentiating feature isn't the one users care about most.

## Assumption Audit

For each direction, list assumptions in three tiers:

### Must Be True (Dealbreakers)
Kill the idea if wrong. Validate before building.
Example: "Users will share their data with us."

### Should Be True (Important)
Significantly impact success; recoverable.
Example: "Users prefer self-serve over talking to a person."

### Might Be True (Nice to Have)
Secondary features / optimizations. Don't validate until core is proven.
Example: "Users will want to share results with teammates."

## Decision Framework

|                    | High Feasibility | Low Feasibility |
|--------------------|-------------------|-----------------|
| **High Value**     | Do this first     | Worth the risk   |
| **Low Value**      | Only if trivial   | Don't do this    |

Use differentiation as tiebreaker within a quadrant.

## MVP Scoping Principles

1. **One job, done well.** The MVP nails one user job, not three.
2. **Riskiest assumption first.** MVP's purpose is to test the bet most likely to be wrong.
3. **Time-box, not feature-list.** "What can we build and test in [N weeks]?" beats "what features do we need?"
4. **"Not Doing" list is mandatory.** Name cuts and why. Prevents scope creep; forces honest prioritization.
5. **If it's not embarrassing, you waited too long.** The first version should feel incomplete to the builder.
