```yaml
number: 20650
title: "[ty] Fix subtyping of invariant generics specialized with `Any`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/non-fully-static-subtyping
created_at: 2025-09-30T16:14:55Z
updated_at: 2025-10-01T10:15:27Z
url: https://github.com/astral-sh/ruff/pull/20650
synced_at: 2026-01-12T15:57:07Z
```

# [ty] Fix subtyping of invariant generics specialized with `Any`

---

_@AlexWaygood_

## Summary

Currently we consider `list[Any]` a subtype of `list[Any]`. But we shouldn't, for the same reason that we shouldn't consider `Any` a subtype of `Any`: the two `Any`s might materialize to different types.

The reason why we make this mistake on `main` is that we (correctly) consider `list[Any]` to be equivalent to `list[Any]`, but then incorrectly assume that if `T` is equivalent to `S` then `T` will necessarily be a subtype of `S`.

## Test Plan

- Added mdtests that fail on `main`.
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable`


---

_Review requested from @carljm by @AlexWaygood on 2025-09-30 16:14_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-30 16:14_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-30 16:14_

---

_Label `bug` added by @AlexWaygood on 2025-09-30 16:14_

---

_Label `ty` added by @AlexWaygood on 2025-09-30 16:14_

---

_Comment by @github-actions[bot] on 2025-09-30 16:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-30 16:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/json_util.py:1015:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, (Any, JSONOptions, /) -> Any].__setitem__(key: int, value: (Any, JSONOptions, /) -> Any, /) -> None` cannot be called with a key of type `object` and a value of type `((Any, JSONOptions, /) -> Any) | ((obj: Any, dummy0: Any) -> Any) | ((obj: Binary, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: datetime, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Any, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: int | float, json_options: JSONOptions) -> Any) | ((obj: int, json_options: JSONOptions) -> Any) | ((obj: UUID, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Int64, json_options: JSONOptions) -> Any) | ((obj: Code, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: DBRef, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((dummy0: Any, dummy1: Any) -> dict[Unknown, Unknown]) | ((obj: ObjectId, dummy0: Any) -> dict[Unknown, Unknown]) | ((obj: Timestamp, dummy0: Any) -> dict[Unknown, Unknown])` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`
+ bson/json_util.py:1015:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, (Any, JSONOptions, /) -> Any].__setitem__(key: int, value: (Any, JSONOptions, /) -> Any, /) -> None` cannot be called with a key of type `object` and a value of type `((Any, JSONOptions, /) -> Any) | ((obj: Any, dummy0: Any) -> Any) | ((obj: bytes, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: datetime, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Any, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: int | float, json_options: JSONOptions) -> Any) | ((obj: int, json_options: JSONOptions) -> Any) | ((obj: UUID, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Binary, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Int64, json_options: JSONOptions) -> Any) | ((obj: Code, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: DBRef, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((dummy0: Any, dummy1: Any) -> dict[Unknown, Unknown]) | ((obj: ObjectId, dummy0: Any) -> dict[Unknown, Unknown]) | ((obj: Timestamp, dummy0: Any) -> dict[Unknown, Unknown])` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `bound method type[Index[Any]].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> Index[Any]`
+ static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `(bound method type[Index[Any]].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> Index[Any]) | (bound method <class 'Index'>.from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> Index[Any])`

jax (https://github.com/google/jax)
- jax/_src/interpreters/partial_eval.py:2513:15: warning[possibly-missing-attribute] Attribute `frame` on type `Trace[Unknown] | Unknown` may be missing
+ jax/_src/interpreters/partial_eval.py:2513:15: warning[possibly-missing-attribute] Attribute `frame` on type `Trace[Unknown] | Unknown | DynamicJaxprTrace` may be missing

```
</details>
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-09-30 16:28_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fnon-fully-static-subtyping?runnerMode=WallTime)

### Merging #20650 will **improve performances by 68.52%**

<sub>Comparing <code>alex/non-fully-static-subtyping</code> (e790951) with <code>main</code> (d9473a2)</sub>



### Summary

`⚡ 1` improvement  
`✅ 7` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` small[freqtrade] `` | 9.2 s | 5.5 s | +68.52% |


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:620 on 2025-09-30 17:22_

This might deserve a comment why we can't use `when_equivalent_to` here. (I was confused at first, at least.) My understanding is that it's because `when_equivalent_to` specifically checks _gradual_ equivalence, which won't give the right answer for `TypeRelation::Subtyping`. (If that's right, it suggests that the old code was wrong for the `Subtyping` case?)

---

_@dcreager approved on 2025-09-30 17:23_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:620 on 2025-09-30 17:23_

(Or maybe based on your Discord comment it's because it's faster? That would also deserve a comment)

---

_@dcreager reviewed on 2025-09-30 17:23_

---

_@AlexWaygood reviewed on 2025-09-30 17:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:620 on 2025-09-30 17:35_

Heh, no, the motivation was correctness and the speed improvement was a pleasant surprise!!

---

_Merged by @AlexWaygood on 2025-10-01 10:05_

---

_Closed by @AlexWaygood on 2025-10-01 10:05_

---

_Branch deleted on 2025-10-01 10:05_

---
