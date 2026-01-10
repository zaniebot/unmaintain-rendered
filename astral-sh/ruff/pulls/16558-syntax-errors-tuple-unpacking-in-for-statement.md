```yaml
number: 16558
title: "[syntax-errors] Tuple unpacking in `for` statement iterator clause before Python 3.9"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-unpacking-for-loop
created_at: 2025-03-07T19:42:43Z
updated_at: 2025-03-13T19:55:20Z
url: https://github.com/astral-sh/ruff/pull/16558
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] Tuple unpacking in `for` statement iterator clause before Python 3.9

---

_Pull request opened by @ntBre on 2025-03-07 19:42_

Summary
--

This PR reuses a slightly modified version of the `check_tuple_unpacking` method added for detecting unpacking in `return` and `yield` statements to detect the same issue in the iterator clause of `for` loops.

I ran into the same issue with a bare `for x in *rest: ...` example (invalid even on Python 3.13) and added it as a comment on https://github.com/astral-sh/ruff/issues/16520.

I considered just making this an additional `StarTupleKind` variant as well, but this change was in a different version of Python, so I kept it separate. 

Test Plan
--

New inline tests.

---

_Label `parser` added by @ntBre on 2025-03-07 19:42_

---

_Label `preview` added by @ntBre on 2025-03-07 19:42_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-07 19:42_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-07 19:42_

---

_Comment by @github-actions[bot] on 2025-03-07 20:06_

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

_@MichaReiser approved on 2025-03-07 20:56_

---

_@dhruvmanila approved on 2025-03-10 11:10_

---

_Merged by @ntBre on 2025-03-13 19:55_

---

_Closed by @ntBre on 2025-03-13 19:55_

---

_Branch deleted on 2025-03-13 19:55_

---
