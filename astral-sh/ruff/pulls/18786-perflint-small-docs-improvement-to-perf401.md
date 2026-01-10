```yaml
number: 18786
title: "[Perflint] Small docs improvement to `PERF401`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: PERF401-docs-cleanup
created_at: 2025-06-19T07:28:09Z
updated_at: 2025-06-20T21:50:52Z
url: https://github.com/astral-sh/ruff/pull/18786
synced_at: 2026-01-10T18:39:08Z
```

# [Perflint] Small docs improvement to `PERF401`

---

_Pull request opened by @MeGaGiGaGon on 2025-06-19 07:28_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

While reading the docs I noticed this paragraph on `PERF401`. It was added in the same PR that the bug with `:=` was fixed, #15050, but don't know why it was added. The fix should already take care of adding the parenthesis, so having this paragraph in the docs is just confusing since it sounds like the user has to do something.

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected


---

_Label `documentation` added by @MichaReiser on 2025-06-20 06:20_

---

_Review requested from @dylwil3 by @MichaReiser on 2025-06-20 06:20_

---

_Comment by @github-actions[bot] on 2025-06-20 06:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-06-20 14:55_

Good catch, thank you!

---

_Merged by @dylwil3 on 2025-06-20 14:55_

---

_Closed by @dylwil3 on 2025-06-20 14:55_

---

_Branch deleted on 2025-06-20 21:50_

---
