```yaml
number: 19127
title: "[`pylint`, `pyupgrade`] Fix syntax errors in examples (`PLW1501`, `UP028`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: make-examples-valid-code
created_at: 2025-07-03T16:16:56Z
updated_at: 2025-07-04T18:47:07Z
url: https://github.com/astral-sh/ruff/pull/19127
synced_at: 2026-01-10T18:33:12Z
```

# [`pylint`, `pyupgrade`] Fix syntax errors in examples (`PLW1501`, `UP028`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-03 16:16_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

From me and @ntBre's discussion in #19111.

This PR makes these two examples into valid code, since they previously had `F701`-`F707` syntax errors. `SIM110` was already fixed in a different PR, I just forgot to pull.

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected.

---

_Comment by @github-actions[bot] on 2025-07-03 16:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-07-04 18:38_

Thanks!

---

_Merged by @dylwil3 on 2025-07-04 18:38_

---

_Closed by @dylwil3 on 2025-07-04 18:38_

---

_Branch deleted on 2025-07-04 18:38_

---

_Label `documentation` added by @dylwil3 on 2025-07-04 18:47_

---
