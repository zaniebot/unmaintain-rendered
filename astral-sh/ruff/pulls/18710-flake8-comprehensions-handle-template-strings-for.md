```yaml
number: 18710
title: "[`flake8-comprehensions`] Handle template strings for comprehension fixes"
type: pull_request
state: merged
author: dylwil3
labels:
  - fixes
  - python314
assignees: []
merged: true
base: main
head: py314/c4
created_at: 2025-06-16T14:37:27Z
updated_at: 2025-06-19T21:23:46Z
url: https://github.com/astral-sh/ruff/pull/18710
synced_at: 2026-01-12T15:56:24Z
```

# [`flake8-comprehensions`] Handle template strings for comprehension fixes

---

_@dylwil3_

Essentially this PR ensures that when we do fixes like this:

```diff
- t"{set(f(x) for x in foo)}"
+ t"{ {f(x) for x in foo} }"
```
we are correctly adding whitespace around the braces. 

This logic is already in place for f-strings and just needed to be generalized to interpolated strings.


---

_Comment by @github-actions[bot] on 2025-06-16 14:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Py314/c4" to "[`flake8-comprehensions`] Handle template strings for comprehension fixes" by @dylwil3 on 2025-06-16 14:50_

---

_Label `python314` added by @dylwil3 on 2025-06-16 14:50_

---

_Label `fixes` added by @dylwil3 on 2025-06-16 14:50_

---

_@dylwil3 reviewed on 2025-06-17 12:44_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/ast/mod.rs`:367 on 2025-06-17 12:44_

While in general it'd be nice to merge cases like this, it's difficult to do here because `TStringValue` and `FStringValue` aren't really merge-able (for the usual reason that t-strings can contain implicitly concatenated f-strings). But should I put a common method on the values to get the first quote style?

---

_Marked ready for review by @dylwil3 on 2025-06-17 12:55_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/ast/mod.rs`:358 on 2025-06-17 13:15_

```suggestion
        // Find the quote character used to start the containing interpolated string.
```

---

_@dylwil3 reviewed on 2025-06-17 13:16_

---

_@MichaReiser approved on 2025-06-18 08:24_

---

_Merged by @dylwil3 on 2025-06-19 21:23_

---

_Closed by @dylwil3 on 2025-06-19 21:23_

---
