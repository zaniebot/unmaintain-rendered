```yaml
number: 16845
title: "[red-knot] Check assignability for two callable types"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-assignability
created_at: 2025-03-19T13:23:04Z
updated_at: 2025-03-22T20:58:46Z
url: https://github.com/astral-sh/ruff/pull/16845
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Check assignability for two callable types

---

_Pull request opened by @dhruvmanila on 2025-03-19 13:23_

## Summary

Part of #15382

This PR adds support for checking the assignability of two general callable types.

This is built on top of #16804 by including the gradual parameters check and accepting a function that performs the check between the two types.

## Test Plan

Update `is_assignable_to.md` with callable types section.

---

_Label `red-knot` added by @dhruvmanila on 2025-03-19 13:23_

---

_Comment by @github-actions[bot] on 2025-03-19 13:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
isort (https://github.com/pycqa/isort)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:55:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:66:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:268:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
- Found 81 diagnostics
+ Found 78 diagnostics

```
</details>


---

_Comment by @dhruvmanila on 2025-03-21 10:47_

The ecosystem changes looks correct, they're all related to assigning a `lambda` expression to a function parameter that's annotated with `typing.Callable`.

---

_Marked ready for review by @dhruvmanila on 2025-03-21 10:47_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-21 10:47_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-21 10:47_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-21 10:47_

---

_Review requested from @dcreager by @dhruvmanila on 2025-03-21 10:47_

---

_@carljm approved on 2025-03-21 19:02_

Excellent!

---

_Merged by @dhruvmanila on 2025-03-22 20:58_

---

_Closed by @dhruvmanila on 2025-03-22 20:58_

---

_Branch deleted on 2025-03-22 20:58_

---
