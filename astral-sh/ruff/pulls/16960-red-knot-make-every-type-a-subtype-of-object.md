```yaml
number: 16960
title: "[red-knot] Make every type a subtype of object"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: any-type-is-object-sub-type
created_at: 2025-03-25T00:08:34Z
updated_at: 2025-03-27T14:34:51Z
url: https://github.com/astral-sh/ruff/pull/16960
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Make every type a subtype of object

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Mainly for partially fixing #16953

## Test Plan

Update is_subtype tests. And should maybe do these checks for many other types (is subtype of object but object is not subtype)


---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-25 00:08_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-25 00:08_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-25 00:08_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-25 00:08_

---

_Comment by @github-actions[bot] on 2025-03-25 00:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dhruvmanila approved on 2025-03-27 14:20_

Thanks! I've moved the branch below `Dynamic` and `Never` mainly to make sure we still panic for non-static types.

---

_Label `red-knot` added by @dhruvmanila on 2025-03-27 14:21_

---

_Merged by @dhruvmanila on 2025-03-27 14:24_

---

_Closed by @dhruvmanila on 2025-03-27 14:24_

---

_Branch deleted on 2025-03-27 14:34_

---
