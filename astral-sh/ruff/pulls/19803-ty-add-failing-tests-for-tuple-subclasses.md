```yaml
number: 19803
title: "[ty] Add failing tests for tuple subclasses"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-subclass-tests
created_at: 2025-08-07T11:53:52Z
updated_at: 2025-08-07T13:12:31Z
url: https://github.com/astral-sh/ruff/pull/19803
synced_at: 2026-01-12T15:56:47Z
```

# [ty] Add failing tests for tuple subclasses

---

_@AlexWaygood_

## Summary

This PR adds tests for tuple subclasses. The tests nearly all fail for now (there are many TODO comments added!) but #19669 makes them pass.

I'm extracting the tests into a standalone PR to make #19669 easier to review.

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-08-07 11:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-07 11:53_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-07 11:53_

---

_Label `testing` added by @AlexWaygood on 2025-08-07 11:53_

---

_Label `ty` added by @AlexWaygood on 2025-08-07 11:53_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:402 on 2025-08-07 12:40_

Out of curiosity, which part needs 3.11?

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/unpacking.md`:435 on 2025-08-07 12:46_

```suggestion
    # TODO: should emit a diagnostic ([invalid-assignment] "Too many values to unpack: Expected 2")
```

---

_@dcreager approved on 2025-08-07 12:47_

---

_@AlexWaygood reviewed on 2025-08-07 13:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:402 on 2025-08-07 13:05_

`t2: tuple[int, *tuple[str, ...]]` is invalid syntax on Python <=3.10 -- starred elements are never allowed in annotations on older versions of Python. On Python <=3.10, you would have to use `t1: tuple[int, Unpack[tuple[str, ...]]]` (because there's no backwards compatibility problem that can't be solved with more "bracketese"!!) to express this type, but we don't support `Unpack` yet

---

_Merged by @AlexWaygood on 2025-08-07 13:11_

---

_Closed by @AlexWaygood on 2025-08-07 13:11_

---

_Branch deleted on 2025-08-07 13:11_

---

_@dcreager reviewed on 2025-08-07 13:12_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:402 on 2025-08-07 13:12_

Starred tuples, yes! That paged out of my memory quickly :grimacing: 

---
