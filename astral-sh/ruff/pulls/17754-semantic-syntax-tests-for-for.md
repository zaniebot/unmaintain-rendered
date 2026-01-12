```yaml
number: 17754
title: "[semantic-syntax-tests] for for `InvalidStarExpression`, `DuplicateMatchKey`, and `DuplicateMatchClassAttribute`"
type: pull_request
state: merged
author: maxmynter
labels:
  - testing
assignees: []
merged: true
base: main
head: semantic-coverage-v2
created_at: 2025-05-01T03:31:58Z
updated_at: 2025-05-05T17:32:50Z
url: https://github.com/astral-sh/ruff/pull/17754
synced_at: 2026-01-12T15:56:05Z
```

# [semantic-syntax-tests] for for `InvalidStarExpression`, `DuplicateMatchKey`, and `DuplicateMatchClassAttribute`

---

_@maxmynter_

Re: #17526 

## Summary
Add integration tests for Python Semantic Syntax for `InvalidStarExpression`, `DuplicateMatchKey`, and `DuplicateMatchClassAttribute`.

## Note
- Red knot integration tests for `DuplicateMatchKey` exist already in line 89-101.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
This is a test.
<!-- How was it tested? -->


---

_Review requested from @carljm by @maxmynter on 2025-05-01 03:31_

---

_Review requested from @AlexWaygood by @maxmynter on 2025-05-01 03:31_

---

_Review requested from @sharkdp by @maxmynter on 2025-05-01 03:31_

---

_Review requested from @dcreager by @maxmynter on 2025-05-01 03:31_

---

_Comment by @github-actions[bot] on 2025-05-01 03:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "(tests) Add integration tests for Python Semantic Syntax" to "[semantic-syntax-tests] Add integration tests for Python Semantic Syntax" by @maxmynter on 2025-05-01 03:36_

---

_Renamed from "[semantic-syntax-tests] Add integration tests for Python Semantic Syntax" to "[semantic-syntax-tests] for for `InvalidStarExpression`, `DuplicateMatchKey`, and `DuplicateMatchClassAttribute`" by @maxmynter on 2025-05-01 03:37_

---

_Comment by @github-actions[bot] on 2025-05-01 03:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `testing` added by @MichaReiser on 2025-05-01 06:06_

---

_@carljm approved on 2025-05-01 17:09_

red-knot tests look good

---

_Review requested from @ntBre by @MichaReiser on 2025-05-03 20:30_

---

_@ntBre approved on 2025-05-05 16:13_

Thanks!

---

_Comment by @ntBre on 2025-05-05 16:15_

I rebased to pick up the `red-knot` -> `ty` name change. Will merge once CI passes!

---

_Comment by @ntBre on 2025-05-05 16:25_

Ah, I forgot we changed the error message in https://github.com/astral-sh/ruff/pull/17772 too. I'll push one more update.

---

_Merged by @ntBre on 2025-05-05 17:30_

---

_Closed by @ntBre on 2025-05-05 17:30_

---
