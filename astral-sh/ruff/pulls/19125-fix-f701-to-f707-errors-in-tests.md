```yaml
number: 19125
title: Fix F701 to F707 errors in tests
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - internal
assignees: []
merged: true
base: main
head: fix-F701-to-F707-errors-in-tests
created_at: 2025-07-03T15:53:20Z
updated_at: 2025-07-04T18:47:21Z
url: https://github.com/astral-sh/ruff/pull/19125
synced_at: 2026-01-10T18:33:12Z
```

# Fix F701 to F707 errors in tests

---

_Pull request opened by @MeGaGiGaGon on 2025-07-03 15:53_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Per @ntBre in https://github.com/astral-sh/ruff/pull/19111, it would be a good idea to make the tests no longer have these syntax errors, so this PR updates the tests and snapshots.

`B031` gave me a lot of trouble since the ending test of declaring a function named `groupby` makes it so that inside other functions, it's unclear which `groupby` is referred to since it depends on when the function is called. To fix it I made each function have it's own `from itertools import groupby` so there's no more ambiguity.

## Test Plan

<!-- How was it tested? -->

Updated tests and snapshots.

---

_Comment by @github-actions[bot] on 2025-07-03 16:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-07-04 18:43_

thank you and good catch on B031!

---

_Merged by @dylwil3 on 2025-07-04 18:43_

---

_Closed by @dylwil3 on 2025-07-04 18:43_

---

_Branch deleted on 2025-07-04 18:43_

---

_Label `internal` added by @dylwil3 on 2025-07-04 18:47_

---
