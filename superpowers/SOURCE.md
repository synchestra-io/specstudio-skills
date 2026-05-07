# Source provenance

Imported **2026-04-19** from the `synchestra` branch of
`synchestra-io/synchestra-superpowers` (repo deleted from GitHub the
same day).

- **Source commit tip:** `82eb34c` — *"fix: replace stale install-synchestra references with onboarding in task-board and deviation-report"*
- **Import method:** `git archive synchestra | tar -x` — tracked files only, no git history, no untracked cruft
- **Size:** 142 files, ~1.1M

## Why this lives here

`synchestra-superpowers` was planned as a superpowers clone adapted for
the Synchestra ecosystem. Its direction was superseded by this
SpecStudio plugin — see
[`synchestra-marketing` / ecosystem / VISION.md](https://github.com/synchestra-io/synchestra-marketing/blob/main/ecosystem/VISION.md)
and the
[SpecStudio revisit resolution](https://github.com/synchestra-io/synchestra-marketing/blob/main/decisions/2026-04-19-spec-studio-revisit-resolution.md).

The content is imported here for reuse, reference, and harvesting into
SpecStudio's own skills.

## Caveats

- **Git history is not preserved** — this was a plain file import, not a subtree merge. The 17 local-only commits that existed on the source `synchestra` branch no longer exist anywhere.
- **Nested `.github/workflows/` do not run.** GitHub Actions only fire from repo-root `.github/`, so the workflow files under this directory are inert content, not active automation.
- **Nested `.gitignore` / `.gitattributes`** apply only within this subdirectory (git honors nested ignore files); they do not affect the rest of SpecStudio.
