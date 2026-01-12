```yaml
number: 15061
title: Disable actionlint hook by default when running pre-commit locally
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/speedup-pre-commit
created_at: 2024-12-19T12:31:11Z
updated_at: 2024-12-19T12:49:33Z
url: https://github.com/astral-sh/ruff/pull/15061
synced_at: 2026-01-12T15:55:50Z
```

# Disable actionlint hook by default when running pre-commit locally

---

_@AlexWaygood_

## Summary

We added actionlint as a pre-commit hook in https://github.com/astral-sh/ruff/pull/15021. Unfortunately, it's by far the slowest hook in our pre-commit config, so this now makes running `uvx pre-commit run -a` locally quite slow.

This PR tweaks our pre-commit config so that it will now be disabled by default when running pre-commit locally but will be enabled when pre-commit is run in CI. This is achieved by reworking the hook entry so that it is only run when `--hook-stage=manual` is specified on the command line.

## Test Plan

The hook now runs instantaneously when `uvx pre-commit run -a` is invoked for me locally (because it is skipped), but still picks up errors in GitHub Actions workflows when `--hook-stage=manual` is specified. I also checked the exact command we'll be running in `.github/workflows/ci.yaml`, and actionlint correctly flags issues locally when that command is run.

---

_Label `ci` added by @AlexWaygood on 2024-12-19 12:31_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-19 12:31_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-12-19 12:31_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:589 on 2024-12-19 12:33_

Nit: It wasn't very clear to me what *manual* means. What would you think of renaming it to `CI`?

---

_@MichaReiser approved on 2024-12-19 12:33_

Thank you!

---

_@AlexWaygood reviewed on 2024-12-19 12:37_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:589 on 2024-12-19 12:37_

Unfortunately this is out of my control 😆 it has to be one of git's recognised hook stages or `manual`: https://pre-commit.com/#confining-hooks-to-run-at-certain-stages

---

_@AlexWaygood reviewed on 2024-12-19 12:38_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:589 on 2024-12-19 12:38_

I'll add a comment to this workflow file, though

---

_Comment by @github-actions[bot] on 2024-12-19 12:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @AlexWaygood on 2024-12-19 12:45_

---

_Closed by @AlexWaygood on 2024-12-19 12:45_

---

_Branch deleted on 2024-12-19 12:45_

---
