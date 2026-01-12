```yaml
number: 18368
title: "[ty] Callable types are disjoint from non-callable `@final` nominal instance types"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: ty-intersection-callable-final
created_at: 2025-05-29T12:08:07Z
updated_at: 2025-06-02T11:14:59Z
url: https://github.com/astral-sh/ruff/pull/18368
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Callable types are disjoint from non-callable `@final` nominal instance types

---

_@InSyncWithFoo_

## Summary

Resolves [#513](https://github.com/astral-sh/ty/issues/513).

Callable types are now considered to be disjoint from nominal instance types where:

* The class is `@final`, and
* Its `__call__` either does not exist or is not assignable to `(...) -> Unknown`.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-05-29 12:08_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-05-29 12:08_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-05-29 12:08_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-05-29 12:08_

---

_Label `ty` added by @AlexWaygood on 2025-05-29 12:10_

---

_Comment by @github-actions[bot] on 2025-05-29 12:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @InSyncWithFoo on 2025-05-29 12:12_

I came across this issue while working on #18294:

<pre><code>graphql-core (https://github.com/graphql-python/graphql-core)
- error[invalid-argument-type] <a href="https://github.com/graphql-python/graphql-core/blob/HEAD/src/graphql/execution/execute.py#L1901C9-L1901C10">src/graphql/execution/execute.py:1901:9</a>: Argument to function `experimental_execute_incrementally` is incorrect: Expected `((Any, /) -> bool) | None`, found `bool | None | (def assume_not_awaitable(_value: Any) -> bool)`
+ error[invalid-argument-type] <a href="https://github.com/graphql-python/graphql-core/blob/HEAD/src/graphql/execution/execute.py#L1901C9-L1901C10">src/graphql/execution/execute.py:1901:9</a>: Argument to function `experimental_execute_incrementally` is incorrect: Expected `((Any, /) -> bool) | None`, found `(bool & ((...) -> object)) | None | (def assume_not_awaitable(_value: Any) -> bool)`</code></pre>

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2182 on 2025-05-29 23:23_

```suggestion
                    // TODO: ideally this would check disjointness of the `__call__` signature and the callable signature
                    Symbol::Type(ty, _) => !ty.is_assignable_to(db, CallableType::unknown(db)),
```

---

_@carljm approved on 2025-05-29 23:24_

Thank you!

---

_Merged by @carljm on 2025-05-29 23:27_

---

_Closed by @carljm on 2025-05-29 23:27_

---

_@InSyncWithFoo reviewed on 2025-05-30 01:57_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/types.rs`:2182 on 2025-05-30 01:57_

Isn't it the case that two callable types are never disjoint from each other?

---

_Branch deleted on 2025-05-30 02:03_

---

_@carljm reviewed on 2025-05-30 02:04_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2182 on 2025-05-30 02:04_

Hmm, on second thought I think you are right. Any two different return types can both be inhabited by a callable returning `Never`; any two different signatures can be inhabited by a callable with a sufficiently relaxed signature (and/or overloads).

Sorry for adding the bad comment, I'll make a follow-up tomorrow to remove it.

---

_@AlexWaygood reviewed on 2025-05-30 12:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2182 on 2025-05-30 12:29_

I'm removing the bad comment in https://github.com/astral-sh/ruff/pull/18384

---

_Label `bug` added by @dhruvmanila on 2025-06-02 11:14_

---
