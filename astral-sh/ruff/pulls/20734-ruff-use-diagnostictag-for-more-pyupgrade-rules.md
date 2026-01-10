```yaml
number: 20734
title: "[`ruff`] Use `DiagnosticTag` for more pyupgrade rules"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: add-DiagnosticTag-to-pyupgrade-rules
created_at: 2025-10-07T06:29:04Z
updated_at: 2025-10-08T04:52:43Z
url: https://github.com/astral-sh/ruff/pull/20734
synced_at: 2026-01-10T17:34:34Z
```

# [`ruff`] Use `DiagnosticTag` for more pyupgrade rules

---

_Pull request opened by @TaKO8Ki on 2025-10-07 06:29_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes a part of #20590

Rolling out DiagnosticTag to every existing rule in one go would complicate the review, so in this pull request I’ve limited the changes to **pyupgrade**. (UP019, UP021, UP005, UP026, UP023)

If you’d prefer I include more changes in one go, I can do that—please let me know.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-10-07 06:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `diagnostics` added by @MichaReiser on 2025-10-08 04:51_

---

_@MichaReiser approved on 2025-10-08 04:52_

Nice, thank you

---

_Merged by @MichaReiser on 2025-10-08 04:52_

---

_Closed by @MichaReiser on 2025-10-08 04:52_

---
