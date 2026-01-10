```yaml
number: 17620
title: "[red-knot] fix inheritance-cycle detection for generic classes"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/gencyclemro
created_at: 2025-04-25T00:59:42Z
updated_at: 2025-04-25T17:11:15Z
url: https://github.com/astral-sh/ruff/pull/17620
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] fix inheritance-cycle detection for generic classes

---

_Pull request opened by @carljm on 2025-04-25 00:59_

## Summary

The `ClassLiteralType::inheritance_cycle` method is intended to detect inheritance cycles that would result in cyclic MROs, emit a diagnostic, and skip actually trying to create the cyclic MRO, falling back to an "error" MRO instead with just `Unknown` and `object`.

This method didn't work properly for generic classes. It used `fully_static_explicit_bases`, which filter-maps `explicit_bases` over `Type::into_class_type`, which returns `None` for an unspecialized generic class literal. So in a case like `class C[T](C): ...`, because the explicit base is an unspecialized generic, we just skipped it, and failed to detect the class as cyclically defined.

Instead, iterate directly over all `explicit_bases`, and explicitly handle both the specialized (`GenericAlias`) and unspecialized (`ClassLiteral`) cases, so that we check all bases and correctly detect cyclic inheritance.

## Test Plan

Added mdtests.


---

_Label `red-knot` added by @carljm on 2025-04-25 00:59_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-25 00:59_

---

_Review requested from @sharkdp by @carljm on 2025-04-25 00:59_

---

_Review requested from @dcreager by @carljm on 2025-04-25 00:59_

---

_Comment by @github-actions[bot] on 2025-04-25 01:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-04-25 13:40_

---

_Merged by @carljm on 2025-04-25 13:55_

---

_Closed by @carljm on 2025-04-25 13:55_

---

_Branch deleted on 2025-04-25 13:55_

---

_@dcreager reviewed on 2025-04-25 17:11_

Thanks for catching this! I like how simple the fix turned out to be

---
