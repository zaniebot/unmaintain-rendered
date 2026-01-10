```yaml
number: 22378
title: "[ty] Support `dataclass_transform` as a function call"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: charlie/class-x
created_at: 2026-01-04T23:14:59Z
updated_at: 2026-01-10T13:45:46Z
url: https://github.com/astral-sh/ruff/pull/22378
synced_at: 2026-01-10T15:56:07Z
```

# [ty] Support `dataclass_transform` as a function call

---

_Pull request opened by @charliermarsh on 2026-01-04 23:14_

## Summary

Instead of just as a decorator.

Closes https://github.com/astral-sh/ty/issues/2319.


---

_Label `bug` added by @charliermarsh on 2026-01-04 23:15_

---

_Label `ty` added by @charliermarsh on 2026-01-04 23:15_

---

_Comment by @astral-sh-bot[bot] on 2026-01-04 23:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-04 23:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_annotations.py:644:13: error[invalid-assignment] Object of type `<decorator produced by dataclass-like function>` is not assignable to `<class 'C'>`
+ tests/test_annotations.py:644:13: error[invalid-assignment] Object of type `<class 'tests.test_annotations.TestAnnotations.<locals of function 'test_init_type_hints_fake_module'>.C @ tests/test_annotations.py:640'>` is not assignable to `<class 'tests.test_annotations.TestAnnotations.<locals of function 'test_init_type_hints_fake_module'>.C @ tests/test_annotations.py:640'>`
- tests/test_make.py:620:13: error[invalid-assignment] Object of type `<decorator produced by dataclass-like function>` is not assignable to `<class 'C'>`
+ tests/test_make.py:620:13: error[invalid-assignment] Object of type `<class 'tests.test_make.TestAttributes.<locals of function 'test_adds_all_by_default'>.C @ tests/test_make.py:615'>` is not assignable to `<class 'tests.test_make.TestAttributes.<locals of function 'test_adds_all_by_default'>.C @ tests/test_make.py:615'>`
+ tests/test_make.py:675:13: error[invalid-assignment] Object of type `<class 'tests.test_make.TestAttributes.<locals of function 'test_respects_init_attrs_init'>.C @ tests/test_make.py:672'>` is not assignable to `<class 'tests.test_make.TestAttributes.<locals of function 'test_respects_init_attrs_init'>.C @ tests/test_make.py:672'>`
+ tests/test_make.py:2872:17: error[invalid-assignment] Object of type `type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858] | type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858]` is not assignable to `<class 'tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858'>`
+ tests/test_make.py:2882:16: error[unsupported-operator] Operator `<` is not supported between two objects of type `C | @Todo`
+ tests/test_make.py:2887:16: error[unsupported-operator] Operator `>` is not supported between two objects of type `C | @Todo`
+ tests/test_slots.py:217:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`
+ tests/test_slots.py:222:9: error[unresolved-attribute] Unresolved attribute `t` on type `SimpleOrdinaryClass`.
+ tests/test_slots.py:224:17: error[invalid-argument-type] Argument to bound method `method` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`
+ tests/test_slots.py:225:27: error[invalid-argument-type] Argument to bound method `classmethod` is incorrect: Expected `type[tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193]`, found `type[tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193]`
+ tests/test_slots.py:228:50: error[unresolved-attribute] Class `SimpleOrdinaryClass` has no attribute `__slots__`
+ tests/test_slots.py:230:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`
+ tests/test_slots.py:232:11: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`, found `tests.test_slots.<locals of function 'test_nonslots_these'>.SimpleOrdinaryClass @ tests/test_slots.py:193`
- Found 616 diagnostics
+ Found 627 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- Found 1840 diagnostics
+ Found 1839 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2026-01-05 00:46_

(The `hydra-zen` diagnostic needs work.)

---

_@charliermarsh reviewed on 2026-01-05 01:26_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/call/bind.rs`:1220 on 2026-01-05 01:26_

I think this is wrong. It's intended to address cases like:
```python
@dataclass_transform()
def hydrated_dataclass(target: type, *, frozen: bool = False) -> Callable[[type[T]], type[T]]:
    def decorator(cls: type[T]) -> type[T]:
        return cls
    return decorator

@hydrated_dataclass(SomeConfig, frozen=True)
class MyConfig:
    pass
```

If we make this change _without_ this declared return type handling, then `hydrated_dataclass(SomeConfig, frozen=True)` returns `type[SomeConfig]` with dataclass params, and we apply _that_ as a decorator.

---

_Marked ready for review by @charliermarsh on 2026-01-05 01:28_

---

_Review requested from @carljm by @charliermarsh on 2026-01-05 01:28_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-05 01:28_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-05 01:28_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-05 01:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:1124 on 2026-01-10 00:46_

I'm impressed that you support this, but I'm curious where it came up? Did you see this in the ecosystem?

This won't work at runtime with stdlib dataclass -- it expects to get a class object, not a `typing.GenericAlias`. But I suppose there could be third-party dataclass-transforms that are built to handle `GenericAlias` at runtime?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:1144 on 2026-01-10 00:50_

What about then using this like `MyClass = decorator(SomeClass)`?

Doesn't seem to work, even on this branch:

```py
class M1:
    x: int

M2 = decorator(M1)

reveal_type(M2)  # reveals `type[M1] & Any`
```

And I'm not sure where the `& Any` comes from?

(As noted above, I don't think any other type checker supports this non-decorator use of a dataclass transform at all, so I don't know that we need to support this edge case, just curious why it doesn't work when the base case above does.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:1056 on 2026-01-10 00:53_

Doesn't seem like any other type checkers support this, but it's cool that we can.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:752 on 2026-01-10 01:22_

lol, oops

I guess no other caller cared about the ordering here?

Looking at the usage in `infer_class_definition`, I think we might need a `.rev()` call there also, to make the behavior match the comment?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1220 on 2026-01-10 01:25_

This is an unfortunate ambiguity baked directly into the `dataclass_transform` spec. It doesn't clarify how we are supposed to tell whether the `@dataclass_transform` decorated function is a decorator or a decorator factory (yet it clearly specifies that both should be supported). I think the spec's assumption is that it doesn't matter, because only decorator-syntax usage (with `@`) is supported, and in that case you can tell how it is used (are there parentheses or not). But we are trying to do something more sophisticated here, and I think it means we have to resort to some kind of heuristic.

But I'm not sure the heuristic should depend on the annotated return type like this; there's no real requirement to annotate your dataclass_transform decorator at all. I think a better heuristic might be based solely on the number, kind, and type of arguments. If only one positional argument is given and it's a class type, assume we should decorate that class. Otherwise, assume we are returning the real decorator.

---

_@carljm approved on 2026-01-10 01:28_

This looks pretty good! We are definitely being more ambitious than other type checkers here (and more ambitious than the spec requires), by supporting non-decorator-syntax usages of `dataclass_transform` at all. Our initial implementation of `dataclass_transform` kind of set us on this path, by implementing the logic generally in function-call-binding and having a "DataclassTransformer" type, rather than doing everything as a special case in decorator application. And this PR seems pretty good, so I don't see much harm in continuing on this path -- it's cool if we can support non-decorator-syntax usage like this.

A few comments inline.

---

_@carljm reviewed on 2026-01-10 01:30_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1258 on 2026-01-10 01:30_

Worth adding a `ClassLiteral::with_dataclass_params` to DRY this up a bit? We're mentioning a lot of extraneous field names here.

---

_@charliermarsh reviewed on 2026-01-10 04:14_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:1124 on 2026-01-10 04:14_

I honestly can't remember -- I may have asked Claude to add something to verify that we preserve specialization after seeing handling for it in the code?!

---

_@charliermarsh reviewed on 2026-01-10 04:14_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/call/bind.rs`:1220 on 2026-01-10 04:14_

(Okay this is very comforting to hear, haha.)

---

_@charliermarsh reviewed on 2026-01-10 04:44_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/call/bind.rs`:1220 on 2026-01-10 04:44_

I think that wouldn't work for this case, since both functions take a single class as its positional arguments:
```python
from typing_extensions import dataclass_transform

@dataclass_transform()
def hydrated_dataclass[T](target: type[T], *, frozen: bool = False):
    def decorator[U](cls: type[U]) -> type[U]:
        return cls
    return decorator
```

For now, I combined the two heuristics.

---

_Merged by @charliermarsh on 2026-01-10 13:45_

---

_Closed by @charliermarsh on 2026-01-10 13:45_

---

_Branch deleted on 2026-01-10 13:45_

---
