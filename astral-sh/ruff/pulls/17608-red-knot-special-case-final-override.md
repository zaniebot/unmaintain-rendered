```yaml
number: 17608
title: "[red-knot] Special case `@final`, `@override`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/final-staticmethod-override
created_at: 2025-04-24T13:49:54Z
updated_at: 2025-04-24T21:45:25Z
url: https://github.com/astral-sh/ruff/pull/17608
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Special case `@final`, `@override`

---

_Pull request opened by @dhruvmanila on 2025-04-24 13:49_

## Summary

This PR adds special-casing for `@final` and `@override` decorator for a similar reason as https://github.com/astral-sh/ruff/pull/17591 to support the invalid overload check.

Both `final` and `override` are identity functions which can be removed once `TypeVar` support is added.


---

_Label `red-knot` added by @dhruvmanila on 2025-04-24 13:49_

---

_Comment by @github-actions[bot] on 2025-04-24 16:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "[red-knot] Special case `@final`, `@staticmethod`, `@override`" to "[red-knot] Special case `@final`, `@override`" by @dhruvmanila on 2025-04-24 20:49_

---

_Marked ready for review by @dhruvmanila on 2025-04-24 21:02_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-24 21:02_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-24 21:02_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-24 21:02_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-24 21:02_

---

_Comment by @dhruvmanila on 2025-04-24 21:04_

I removed the `@staticmethod` special-casing because I realized that it would require additional change which could be as small as returning `FunctionLiteral` in the descriptor protocol. I made that as a quick change locally, will try it in a follow-up but I don't want to prioritize that much.

---

_@carljm approved on 2025-04-24 21:18_

---

_Merged by @dhruvmanila on 2025-04-24 21:45_

---

_Closed by @dhruvmanila on 2025-04-24 21:45_

---

_Branch deleted on 2025-04-24 21:45_

---
