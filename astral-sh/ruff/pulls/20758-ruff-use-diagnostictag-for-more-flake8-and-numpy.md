```yaml
number: 20758
title: "[`ruff`] Use DiagnosticTag for more flake8 and numpy rules"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: add-DiagnosticTag-to-flake8-rules
created_at: 2025-10-08T07:22:34Z
updated_at: 2025-10-13T08:29:15Z
url: https://github.com/astral-sh/ruff/pull/20758
synced_at: 2026-01-12T15:57:09Z
```

# [`ruff`] Use DiagnosticTag for more flake8 and numpy rules

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes a part of #20590 and follow up to #20734

Rolling out `DiagnosticTag` to every existing rule in one go would complicate the review, so in this pull request I’ve limited the changes to flake8 and numpy. (NPY001, NPY003, ARG001, ARG002, ARG003, ARG004, ARG005, PT020, PYI057, S306)

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @AlexWaygood by @TaKO8Ki on 2025-10-08 07:22_

---

_Comment by @github-actions[bot] on 2025-10-08 07:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-10-13 08:29_

---

_Label `diagnostics` added by @MichaReiser on 2025-10-13 08:29_

---

_Comment by @MichaReiser on 2025-10-13 08:29_

Thank you

---

_Merged by @MichaReiser on 2025-10-13 08:29_

---

_Closed by @MichaReiser on 2025-10-13 08:29_

---
