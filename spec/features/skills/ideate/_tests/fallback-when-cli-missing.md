# Scenario: Fallback to direct write when specscore CLI is missing

**Validates:** [ideate#ac:cli-vs-fallback](../README.md#ac-cli-vs-fallback)

## Steps

GIVEN the `specscore` binary is NOT on PATH
AND the skill is in Phase 3 with a complete set of inputs
WHEN the skill crystallizes the Idea
THEN the skill writes `spec/ideas/<slug>.md` directly using the authoritative schema documented in `skills/ideate/SKILL.md`
AND the resulting artifact is byte-equivalent (modulo whitespace) to what `specscore new idea <slug>` would have produced
AND `specscore lint spec/ideas/<slug>.md` exits zero when the CLI is later installed and re-run against the same file

GIVEN the fallback path was used
WHEN the user inspects the artifact's front-matter and structure
THEN the front-matter contains all required fields (`type`, `id`, `status`, `date`, `owner`, `promotes_to`, `supersedes`)
AND every required Idea section (`Problem Statement`, `Recommended Direction`, `MVP Scope`, `Not Doing`, `Key Assumptions to Validate`) is present and non-empty

---
*This document follows the https://specscore.md/scenario-specification*
