# Scenarios — `ideate` skill

Scenarios validating the [Ideate Skill feature](../README.md). Each scenario references the AC or REQ it validates and uses `Given / When / Then` form per the [SpecScore Scenario specification](https://specscore.md/scenario-specification).

## Index

| Scenario | Validates |
|---|---|
| [hard-gate-blocks-specify-without-approval.md](hard-gate-blocks-specify-without-approval.md) | `ideate#ac:hard-gate-enforced` |
| [empty-not-doing-rejected.md](empty-not-doing-rejected.md) | `ideate#req:not-doing-required` |
| [docs-ideas-path-rejected.md](docs-ideas-path-rejected.md) | `ideate#req:no-docs-path` |
| [cli-path-preferred-when-available.md](cli-path-preferred-when-available.md) | `ideate#ac:cli-vs-fallback` |
| [fallback-when-cli-missing.md](fallback-when-cli-missing.md) | `ideate#ac:cli-vs-fallback` |
| [approval-emits-idea-approved-event.md](approval-emits-idea-approved-event.md) | `ideate#ac:lifecycle-events` |
| [clear-intent-hands-off-to-specify.md](clear-intent-hands-off-to-specify.md) | `ideate#ac:skip-condition-respected` |
| [manual-promotes-to-edit-rejected.md](manual-promotes-to-edit-rejected.md) | `ideate#ac:promotion-boundary-held` |

---
*This document follows the https://specscore.md/scenarios-index-specification*
