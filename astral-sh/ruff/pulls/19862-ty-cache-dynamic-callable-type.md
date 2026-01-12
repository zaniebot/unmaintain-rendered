```yaml
number: 19862
title: "[ty] Cache dynamic callable type"
type: pull_request
state: closed
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
base: main
head: perf-callable-type
created_at: 2025-08-11T11:33:19Z
updated_at: 2025-11-16T11:47:12Z
url: https://github.com/astral-sh/ruff/pull/19862
synced_at: 2026-01-12T15:56:49Z
```

# [ty] Cache dynamic callable type

---

_@MichaReiser_

## Summary

Cache the `CallableType` for `Type::dynamic` to reduce the congestion around salsa's `intern_id` lock. 

https://github.com/astral-sh/ty/issues/968

This reduces check time on a large project from 25s to 18s on my mac mini. The memory increase should be proportional to the amount of dynamic types (which should be fixed once we eliminated all Todo types)

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-08-11 11:33_

---

_Label `performance` added by @MichaReiser on 2025-08-11 11:33_

---

_Label `ty` added by @MichaReiser on 2025-08-11 11:33_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-11 11:33_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-11 11:33_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-11 11:33_

---

_Comment by @github-actions[bot] on 2025-08-11 11:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-11 11:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6415 on 2025-08-11 11:41_

Caching the method definitely makes sense if it improves performance this much, but I think I'd prefer for this to be a method on `CallableType` rather than a method on `DynamicType`. I'm not sure it makes sense to think of this as transforming a `DynamicType` "into a `CallableType`": what we're really doing is creating a `CallableType` that returns a certain `DynamicType`.

Also, could we add a comment saying why we specifically cache the constructor if we're constructing a `CallableType` that returns a `DynamicType`, rather than generally caching the `CallableType::single()` method?

---

_@AlexWaygood reviewed on 2025-08-11 11:41_

Nice!

---

_Comment by @MichaReiser on 2025-08-11 12:17_

Okay, my latest version undos the entire performance improvement because calling the cached query requires interning on its own, lol

---

_@AlexWaygood reviewed on 2025-08-11 12:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6413 on 2025-08-11 12:32_

```suggestion
impl DynamicType {
```

---

_Comment by @MichaReiser on 2025-08-11 12:59_

This does help performance but there's still a congested lock because non-argument queries in salsa also require interning (it's just that the lock is now sharded by dynamic type which improves performance somewhat). Addressing the todo's will also mean that the locks become more congested again. So maybe this is just not worth it (But the perf improvement is massive, ~40 -> 30s or even less on my linux machine)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8056 on 2025-08-11 13:06_

```suggestion
    /// Each variant uses its own query to achieve an extra degree of sharding (Salsa
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8051 on 2025-08-11 13:07_

```suggestion
    /// Create a callable type with ``*args: Any, **kwargs: Any`` as the parameter spec
    /// and a dynamic return type.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8148 on 2025-08-11 13:14_

I think these should all use `Parameters::gradual_form()` rather than `Parameters::unknown()`, if we want to keep the semantics exactly as they are on `main`. The former creates a `*args: Any, **kwargs: Any` parameter spec, the second creates a `*args: Unknown, **kwargs: Unknown` parameter spec. Practically speaking these are usually treated the same by ty, but `Unknown` normalizes to `Any`, and it could also have an effect on diagnostics messages.

---

_@AlexWaygood reviewed on 2025-08-11 13:15_

---

_Comment by @MichaReiser on 2025-08-11 13:16_

Starring more at this. The sharding in Salsa isn't working because all threads look up the same few `CallableType`,s which, because they're the same, are all stored in the same shard. 

It might be worth to use an `RwLock` in the interning table. But I don't know how easily this is possible, due to the LRU

---

_Comment by @MichaReiser on 2025-08-11 16:17_

@carljm had a much better idea. I'll open a new PR

---

_Comment by @carljm on 2025-08-11 17:07_

I assume this won't show much if any perf impact after the PR that special-cases Dynamic in reachability constraint evaluation?

---

_Comment by @MichaReiser on 2025-08-11 18:11_

> I assume this won't show much if any perf impact after the PR that special-cases Dynamic in reachability constraint evaluation?

No, I wanted to close this PR (I thought I did)

---

_Closed by @MichaReiser on 2025-08-11 18:11_

---

_Branch deleted on 2025-11-16 11:47_

---
