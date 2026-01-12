```yaml
number: 21934
title: "[ty] Use `ParamSpec` without the attr for inferable check"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: dhruv/inferrable-paramspec-check-without-attr
created_at: 2025-12-12T07:49:00Z
updated_at: 2025-12-15T18:36:13Z
url: https://github.com/astral-sh/ruff/pull/21934
synced_at: 2026-01-12T15:57:37Z
```

# [ty] Use `ParamSpec` without the attr for inferable check

---

_@dhruvmanila_

## Summary

fixes: https://github.com/astral-sh/ty/issues/1820

## Test Plan

Add new mdtests.

Ecosystem changes removes all false positives.


---

_Label `bug` added by @dhruvmanila on 2025-12-12 07:49_

---

_Label `ty` added by @dhruvmanila on 2025-12-12 07:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 07:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 07:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/__init__.py:358:14: warning[unused-ignore-comment] Unused `ty: ignore` directive
- Found 172 diagnostics
+ Found 173 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
- src/async_utils/corofunc_cache.py:148:12: error[invalid-return-type] Return type does not match returned value: expected `CoroCacheDeco`, found `def wrapper[**P, R](coro: CoroLike[P@wrapper, R@wrapper], /) -> CoroFunc[P@wrapper, R@wrapper]`
- src/async_utils/corofunc_cache.py:231:12: error[invalid-return-type] Return type does not match returned value: expected `CoroCacheDeco`, found `def wrapper[**P, R](coro: CoroLike[P@wrapper, R@wrapper], /) -> CoroFunc[P@wrapper, R@wrapper]`
- src/async_utils/task_cache.py:213:12: error[invalid-return-type] Return type does not match returned value: expected `TaskCacheDeco`, found `def wrapper[**P, R](coro: TaskCoroFunc[P@wrapper, R@wrapper], /) -> TaskFunc[P@wrapper, R@wrapper]`
- src/async_utils/task_cache.py:301:12: error[invalid-return-type] Return type does not match returned value: expected `TaskCacheDeco`, found `def wrapper[**P, R](coro: TaskCoroFunc[P@wrapper, R@wrapper], /) -> TaskFunc[P@wrapper, R@wrapper]`
- Found 15 diagnostics
+ Found 11 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/lib/interface_ext/__init__.py:61:24: error[invalid-assignment] Object of type `InterfaceImpl` is not assignable to `Interface`
- src/antidote/lib/lazy_ext/__init__.py:29:14: error[invalid-assignment] Object of type `LazyImpl` is not assignable to `Lazy`
- Found 275 diagnostics
+ Found 273 diagnostics

Expression (https://github.com/cognitedata/Expression)
- expression/effect/async_option.py:122:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `AsyncBuilder.__call__`
- expression/effect/async_result.py:123:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `AsyncBuilder.__call__`
- expression/effect/option.py:98:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `Builder.__call__`
- expression/effect/result.py:89:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `Builder.__call__`
- Found 214 diagnostics
+ Found 210 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/resample.py:265:9: error[invalid-method-override] Invalid override of method `pipe`: Definition is incompatible with `BaseGroupBy.pipe`
- pandas/core/window/expanding.py:337:9: error[invalid-method-override] Invalid override of method `pipe`: Definition is incompatible with `RollingAndExpandingMixin.pipe`
- pandas/core/window/rolling.py:2266:9: error[invalid-method-override] Invalid override of method `pipe`: Definition is incompatible with `RollingAndExpandingMixin.pipe`
- Found 3717 diagnostics
+ Found 3714 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @dhruvmanila on 2025-12-12 08:27_

---

_Review requested from @carljm by @dhruvmanila on 2025-12-12 08:27_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-12-12 08:27_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-12-12 08:27_

---

_Review requested from @dcreager by @dhruvmanila on 2025-12-12 08:27_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2193 on 2025-12-13 01:03_

I find it a bit suspicious that this rule is so much simpler than our existing rules for other typevars, which require six or seven different match clauses to implement. It seems like in principle the rules should be the same, the only difference being that we need to account for stripping the attributes in this case.

Is it possible that this could be instead implemented by just checking the attribute is the same, stripping the attribute, and then delegating to a recursive call to `has_relation_to_impl` with the base typevars instead of the attributes?

Alternatively, given the restrictions on where ParamSpec attributes can legally appear (only in a callable signature, and only together, applied to `*args` and `**kwargs`), I wonder if this wouldn't be easier/simpler if we handled it directly in callable-type `has_relation_to_impl`?

If none of that seems to work easily, I think it'd be OK to land this version (ecosystem impact looks good as far as it goes!), but maybe with an added TODO that this probably isn't fully correct yet?

---

_@carljm approved on 2025-12-13 01:03_

Thank you!

---

_@dhruvmanila reviewed on 2025-12-15 04:55_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:2193 on 2025-12-15 04:55_

> Is it possible that this could be instead implemented by just checking the attribute is the same, stripping the attribute, and then delegating to a recursive call to `has_relation_to_impl` with the base typevars instead of the attributes?

Yeah, I tried this but it fails in this test case that I added in this PR:

```py
from typing import Callable

class Container[**P]:
    def method(self, f: Callable[P, None]) -> Callable[P, None]:
        return f

    def try_assign[**Q](self, f: Callable[Q, None]) -> Callable[Q, None]:
        # error: [invalid-return-type] "Return type does not match returned value: expected `(**Q@try_assign) -> None`, found `(**P@Container) -> None`"
        # error: [invalid-argument-type] "Argument to bound method `method` is incorrect: Expected `(**P@Container) -> None`, found `(**Q@try_assign) -> None`"
        return self.method(f)
```

where the `invalid-argument-type` diagnostic is not emitted.

> Alternatively, given the restrictions on where ParamSpec attributes can legally appear (only in a callable signature, and only together, applied to `*args` and `**kwargs`), I wonder if this wouldn't be easier/simpler if we handled it directly in callable-type `has_relation_to_impl`?

Yeah, I think that makes sense. I'll move this check to the `Signature::has_relation_to_inner`

Some additional context here: https://discord.com/channels/1039017663004942429/1449080092515893316/1449975836055703673

---

_Merged by @dhruvmanila on 2025-12-15 05:34_

---

_Closed by @dhruvmanila on 2025-12-15 05:34_

---

_Branch deleted on 2025-12-15 05:34_

---

_@carljm reviewed on 2025-12-15 18:09_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2193 on 2025-12-15 18:09_

Interesting. I think perhaps this reveals a bug in our handling of non-ParamSpec typevars?

```py
class Container[T]:
    def method(self, x: T) -> T:
        return x

    def try_assign[U](self, x: U) -> U:
        return self.method(x)
```
https://play.ty.dev/e477cd72-491d-4953-b77a-97ad0d47fd1f

It seems like there should be two diagnostics on the last line there as well (I wouldn't expect `U` to be assignable to `T` in the call to `self.method`), but we only get one (invalid return type).

---

_@carljm reviewed on 2025-12-15 18:36_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2193 on 2025-12-15 18:36_

Filed https://github.com/astral-sh/ty/issues/1898

---
