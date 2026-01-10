```yaml
number: 17621
title: "[red-knot] fix detecting a metaclass on a not-explicitly-specialized generic base"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/genmeta
created_at: 2025-04-25T01:37:06Z
updated_at: 2025-04-25T13:55:57Z
url: https://github.com/astral-sh/ruff/pull/17621
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] fix detecting a metaclass on a not-explicitly-specialized generic base

---

_Pull request opened by @carljm on 2025-04-25 01:37_

## Summary

After https://github.com/astral-sh/ruff/pull/17620 (which this PR is based on), I was looking at other call sites of `Type::into_class_type`, and I began to feel that _all_ of them were currently buggy due to silently skipping unspecialized generic class literal types (though in some cases the bug hadn't shown up yet because we don't understand legacy generic classes from typeshed), and in every case they would be better off if an unspecialized generic class literal were implicitly specialized with the default specialization (which is the usual Python typing semantics for an unspecialized reference to a generic class), instead of silently skipped.

So I changed the method to implicitly apply the default specialization, and added a test that previously failed for detecting metaclasses on an unspecialized generic base.

I also renamed the method to `to_class_type`, because I feel we have a strong naming convention where `Type::into_foo` is always a trivial `const fn` that simply returns `Some()` if the type is of variant `Foo` and `None` otherwise. Even the existing method (with it handling both `GenericAlias` and `ClassLiteral`, and distinguishing kinds of `ClassLiteral`) was stretching this convention, and the new version definitely breaks that envelope.

## Test Plan

Added a test that failed before this PR.


---

_Label `red-knot` added by @carljm on 2025-04-25 01:37_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-25 01:37_

---

_Review requested from @sharkdp by @carljm on 2025-04-25 01:37_

---

_Review requested from @dcreager by @carljm on 2025-04-25 01:37_

---

_Comment by @github-actions[bot] on 2025-04-25 01:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-04-25 13:42_

---

_Merged by @carljm on 2025-04-25 13:55_

---

_Closed by @carljm on 2025-04-25 13:55_

---

_Branch deleted on 2025-04-25 13:55_

---
