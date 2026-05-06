# Scenario: Manual promotes_to edits are out of scope for ideate

**Validates:** [ideate#ac:promotion-boundary-held](../README.md#ac-promotion-boundary-held)

## Steps

GIVEN an approved Idea exists at `spec/ideas/my-idea.md`
AND the user asks `spec-studio:ideate` to set `promotes_to: [feature-x]` in the front-matter
WHEN the skill processes the request
THEN the skill refuses to edit `promotes_to`
AND the skill explains that `promotes_to` is managed state, populated by Synchestra in response to a Feature declaring `Source Ideas`

GIVEN the user asks `spec-studio:ideate` to scaffold or modify a SpecScore Feature
WHEN the skill processes the request
THEN the skill refuses to write or modify any file under `spec/features/`
AND the skill hands off to `spec-studio:specify` with an explanation

GIVEN a Feature is later created at `spec/features/feature-x/` with `**Source Ideas:** my-idea`
WHEN Synchestra tooling reconciles the link
THEN the Idea's `promotes_to` is populated with `[feature-x]`
AND the Idea's `status` transitions from `Approved` to `Specified`
AND `ideate` is not involved in either change

---
*This document follows the https://specscore.md/scenario-specification*
