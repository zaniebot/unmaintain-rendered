```yaml
number: 20329
title: "[ty] Add more tests for special-cased builtin functions and methods"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/builtin-method-callability
created_at: 2025-09-10T12:48:01Z
updated_at: 2025-09-10T17:08:35Z
url: https://github.com/astral-sh/ruff/pull/20329
synced_at: 2026-01-10T17:46:22Z
```

# [ty] Add more tests for special-cased builtin functions and methods

---

_Pull request opened by @AlexWaygood on 2025-09-10 12:48_

## Summary

This PR adds exhaustive unit tests for whether we understand certain special-cased functions and methods as being assignable to `Callable`, and what we understand their signatures as being.

Many of the assertions in these tests currently fail, and the fact that they fail causes issues for my work-in-progress branch adding proper type checks for protocol assignability/subtyping when a protocol has method members. For example, the fact that we do not currently consider the type of `f.__call__` to be callable (for any given function `f`) means that a naive implementation of type checks for protocols with method members would cause us to think that no function could ever satisfy a protocol with a `__call__` method.

I intend to fix some of these assertions in followup PRs. Also included in this PR is a fix for `PropertyInstanceType` subtyping and equivalence. The current equality-based subtyping logic causes strange `invalid-argument-type` errors on `main` if you try to call `reveal_type(MyClass.my_property.__get__`: https://play.ty.dev/7c0bbfc5-f4e6-41a2-b45f-225fdb1ac1b2).

## Test Plan

This PR is (nearly) all tests


---

_Review requested from @carljm by @AlexWaygood on 2025-09-10 12:48_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-10 12:48_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-10 12:48_

---

_Label `testing` added by @AlexWaygood on 2025-09-10 12:48_

---

_Label `ty` added by @AlexWaygood on 2025-09-10 12:48_

---

_Comment by @github-actions[bot] on 2025-09-10 12:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-10 12:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-09-10 16:48_

---

_Merged by @AlexWaygood on 2025-09-10 17:08_

---

_Closed by @AlexWaygood on 2025-09-10 17:08_

---

_Branch deleted on 2025-09-10 17:08_

---
