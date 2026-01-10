```yaml
number: 16887
title: "[red-knot] Fix gradual equivalence for callable types"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-fix-gradual-equivalence
created_at: 2025-03-21T08:49:53Z
updated_at: 2025-03-24T18:16:08Z
url: https://github.com/astral-sh/ruff/pull/16887
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Fix gradual equivalence for callable types

---

_Pull request opened by @dhruvmanila on 2025-03-21 08:49_

## Summary

As mentioned in https://github.com/astral-sh/ruff/pull/16698#discussion_r2004920075, part of #15382, this PR updates the `is_gradual_equivalent_to` implementation between callable types to be similar to `is_equivalent_to` and checks other attributes of parameters like name, optionality, and parameter kind.

## Test Plan

Expand the existing test cases to consider other properties but not all similar to how the tests are structured for subtyping and assignability.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-21 08:49_

---

_Renamed from "[red-knot] Fix gradual equivalent relation" to "[red-knot] Fix gradual equivalence for callable types" by @dhruvmanila on 2025-03-21 08:50_

---

_Comment by @github-actions[bot] on 2025-03-21 08:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-03-24 16:03_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-24 16:03_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-24 16:03_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-24 16:03_

---

_Review requested from @dcreager by @dhruvmanila on 2025-03-24 16:03_

---

_@carljm approved on 2025-03-24 16:44_

Looks great! I really like the pattern you've established of using a common function with a callback that implements the distinction between subtyping and assignability (or in this case, equivalence and gradual equivalence.)

---

_Merged by @dhruvmanila on 2025-03-24 18:16_

---

_Closed by @dhruvmanila on 2025-03-24 18:16_

---

_Branch deleted on 2025-03-24 18:16_

---
