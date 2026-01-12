```yaml
number: 16135
title: "Make `CallBinding::callable_ty` required"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/binding-callable-ty
created_at: 2025-02-13T09:31:11Z
updated_at: 2025-02-14T07:15:27Z
url: https://github.com/astral-sh/ruff/pull/16135
synced_at: 2026-01-12T15:55:53Z
```

# Make `CallBinding::callable_ty` required

---

_@MichaReiser_

## Summary

The `callable_ty` is always known except in some TODO code where we can use a `TODO` type instead.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-02-13 09:31_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-13 09:31_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-13 09:31_

---

_Label `internal` added by @MichaReiser on 2025-02-13 09:31_

---

_Label `red-knot` added by @MichaReiser on 2025-02-13 09:31_

---

_@MichaReiser reviewed on 2025-02-13 09:32_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:140 on 2025-02-13 09:32_

I wonder if this will be true. E.g. it's already a function in the `Class.__call__` case. So maybe it's always the function type?

---

_@AlexWaygood reviewed on 2025-02-13 12:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:140 on 2025-02-13 12:59_

Not sure I fully understand the question, but it's possible for the `__call__` method to be an instance of another callable object. It doesn't have to be a function (https://bsky.app/profile/alexwaygood.bsky.social/post/3lejkfqwhkk2o)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:140 on 2025-02-14 00:10_

By the time we are actually binding a call, though, we'll have burrowed through all the layers of `__call__` to an actual function.

I think "class" here was meant for the case of instantiating a class, but even in that case the callable type could be considered the `__init__` or `__new__` function we bind to.

This is only used for constructing better error messages, so really how we fill it in depends on what we want our diagnostics to say. If we always put the final function in here, then it might imply that we want functions to carry more of their context, e.g. so we can name it as `C.__call__` instead of just `__call__` in the diagnostic.

---

_@carljm approved on 2025-02-14 00:10_

---

_Merged by @MichaReiser on 2025-02-14 07:15_

---

_Closed by @MichaReiser on 2025-02-14 07:15_

---

_Branch deleted on 2025-02-14 07:15_

---
