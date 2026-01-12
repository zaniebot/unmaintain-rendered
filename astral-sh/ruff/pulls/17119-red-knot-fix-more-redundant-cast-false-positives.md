```yaml
number: 17119
title: "[red-knot] Fix more `redundant-cast` false positives"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/cast-todos
created_at: 2025-04-01T13:04:09Z
updated_at: 2025-04-01T18:03:49Z
url: https://github.com/astral-sh/ruff/pull/17119
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Fix more `redundant-cast` false positives

---

_@AlexWaygood_

## Summary

There are quite a few places we infer `Todo` types currently, and some of them are nested somewhat deeply in type expressions. These can cause spurious issues for the new `redundant-cast` diagnostics. We fixed all the false positives we saw in the mypy_primer report before merging https://github.com/astral-sh/ruff/pull/17100, but I think there are still lots of places where we'd emit false positives due to this check -- we currently don't run on that many projects at all in our mypy_primer check:

https://github.com/astral-sh/ruff/blob/d0c8eaa0923352ccc4ec30c9fac1f573f20998b3/.github/workflows/mypy_primer.yaml#L71

This PR fixes some more false positives from this diagnostic by making the `Type::contains_todo()` method more expansive.

## Test Plan

I added a regression test which causes us to emit a spurious diagnostic on `main`, but does not with this PR.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-01 13:04_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-01 13:04_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-01 13:04_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-01 13:04_

---

_Renamed from "Fix more `redundant-cast` false positives" to "[red-knot] Fix more `redundant-cast` false positives" by @AlexWaygood on 2025-04-01 13:04_

---

_Comment by @github-actions[bot] on 2025-04-01 13:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-04-01 18:01_

---

_Merged by @AlexWaygood on 2025-04-01 18:03_

---

_Closed by @AlexWaygood on 2025-04-01 18:03_

---

_Branch deleted on 2025-04-01 18:03_

---
