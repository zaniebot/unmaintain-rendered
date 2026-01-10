```yaml
number: 17698
title: "[red-knot] Support overloads for callable equivalence"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/overload-equivalence
created_at: 2025-04-29T03:38:16Z
updated_at: 2025-04-29T21:24:01Z
url: https://github.com/astral-sh/ruff/pull/17698
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Support overloads for callable equivalence

---

_Pull request opened by @dhruvmanila on 2025-04-29 03:38_

## Summary

Part of #15383, this PR adds `is_equivalent_to` support for overloaded callables.

This is mainly done by delegating it to the subtyping check in that two types A and B are considered equivalent if A is a subtype of B and B is a subtype of A.

## Test Plan

Add test cases for overloaded callables in `is_equivalent_to.md`


---

_Label `red-knot` added by @dhruvmanila on 2025-04-29 03:38_

---

_Comment by @github-actions[bot] on 2025-04-29 03:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-04-29 14:32_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-29 14:32_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-29 14:32_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-29 14:32_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-29 14:32_

---

_@carljm approved on 2025-04-29 21:00_

Nice!

---

_Merged by @dhruvmanila on 2025-04-29 21:23_

---

_Closed by @dhruvmanila on 2025-04-29 21:23_

---

_Branch deleted on 2025-04-29 21:24_

---
