```yaml
number: 20511
title: "[ty] Match variadic argument to variadic parameter"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: dhruv/match-variadic-parameter
created_at: 2025-09-22T09:22:25Z
updated_at: 2025-09-25T07:51:58Z
url: https://github.com/astral-sh/ruff/pull/20511
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Match variadic argument to variadic parameter

---

_Pull request opened by @dhruvmanila on 2025-09-22 09:22_

## Summary

Closes: https://github.com/astral-sh/ty/issues/1236

This PR fixes a bug where the variadic argument wouldn't match against the variadic parameter in certain scenarios.

This was happening because I didn't realize that the `all_elements` iterator wouldn't keep on returning the variable element (which is correct, I just didn't realize it back then).

I don't think we can use the `resize` method here because we don't know how many parameters this variadic argument is matching against as this is where the actual parameter matching occurs.

## Test Plan

Expand test cases to consider a few more combinations of arguments and parameters which are variadic.


---

_Review requested from @carljm by @dhruvmanila on 2025-09-22 09:22_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-09-22 09:22_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-09-22 09:22_

---

_Review requested from @dcreager by @dhruvmanila on 2025-09-22 09:22_

---

_Label `bug` added by @dhruvmanila on 2025-09-22 09:22_

---

_Label `ty` added by @dhruvmanila on 2025-09-22 09:22_

---

_Comment by @github-actions[bot] on 2025-09-22 09:29_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-22 09:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
ignite (https://github.com/pytorch/ignite)
+ ignite/contrib/engines/tbptt.py:120:28: error[invalid-argument-type] Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `Tbptt_Events`
+ tests/ignite/engine/test_custom_events.py:18:28: error[invalid-argument-type] Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `CustomEvents`
+ tests/ignite/engine/test_custom_events.py:37:28: error[invalid-argument-type] Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `CustomEvents`
+ tests/ignite/engine/test_custom_events.py:103:28: error[invalid-argument-type] Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `CustomEvents`
+ tests/ignite/engine/test_custom_events.py:117:28: error[invalid-argument-type] Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `CustomEvents`
+ tests/ignite/engine/test_custom_events.py:129:32: error[invalid-argument-type] Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `CustomEvents`
+ tests/ignite/engine/test_custom_events.py:140:28: error[invalid-argument-type] Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `CustomEvents`
+ tests/ignite/engine/test_custom_events.py:606:32: error[invalid-argument-type] Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `CustomEvents`
+ tests/ignite/handlers/test_time_profilers.py:71:35: error[invalid-argument-type] Argument to bound method `register_events` is incorrect: Expected `list[str] | list[EventEnum]`, found `CustomEvents`
- Found 2153 diagnostics
+ Found 2162 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/domains/python/_annotations.py:434:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sphinx/domains/python/_annotations.py:498:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sphinx/domains/python/_annotations.py:588:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 522 diagnostics
+ Found 519 diagnostics

meson (https://github.com/mesonbuild/meson)
- test cases/common/214 source set custom target/cp.py:5:10: error[invalid-argument-type] Argument to function `copyfile` is incorrect: Expected `list[str]`, found `str`
- test cases/unit/15 prebuilt object/cp.py:5:10: error[invalid-argument-type] Argument to function `copyfile` is incorrect: Expected `list[str]`, found `str`
- test cases/unit/56 introspection/cp.py:5:10: error[invalid-argument-type] Argument to function `copyfile` is incorrect: Expected `list[str]`, found `str`
- Found 883 diagnostics
+ Found 880 diagnostics

vision (https://github.com/pytorch/vision)
+ test/datasets_utils.py:1045:9: error[invalid-assignment] Object of type `str` is not assignable to `tuple[str, ...]`
- Found 1479 diagnostics
+ Found 1480 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/generation/hypothesis/builder.py:230:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 284 diagnostics
+ Found 285 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 53 diagnostics
+ Found 52 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/generic.py:6089:16: error[no-matching-overload] No overload of function `pipe` matches arguments
- pandas/core/groupby/groupby.py:770:16: error[no-matching-overload] No overload of function `pipe` matches arguments
- pandas/core/window/rolling.py:1573:16: error[no-matching-overload] No overload of function `pipe` matches arguments
- pandas/io/formats/style.py:3961:16: error[no-matching-overload] No overload of function `pipe` matches arguments
- pandas/tests/indexes/interval/test_indexing.py:107:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `Unknown | list[Unknown | int] | list[Unknown | float | int]` does not satisfy constraints (`int`, `int | float`, `Timestamp`, `Timedelta`) of type variable `_OrderableT`
- pandas/tests/indexes/interval/test_indexing.py:110:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `Unknown | list[Unknown | int] | list[Unknown | float | int]` does not satisfy constraints (`int`, `int | float`, `Timestamp`, `Timedelta`) of type variable `_OrderableT`
- Found 3324 diagnostics
+ Found 3318 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/layouts.py:117:26: error[invalid-argument-type] Argument to function `_handle_child_sizing` is incorrect: Expected `list[UIElement]`, found `list[UIElement | list[UIElement]]`
+ src/bokeh/layouts.py:118:16: error[invalid-argument-type] Argument is incorrect: Expected `list[UIElement]`, found `list[UIElement | list[UIElement]]`
+ src/bokeh/layouts.py:152:26: error[invalid-argument-type] Argument to function `_handle_child_sizing` is incorrect: Expected `list[UIElement]`, found `list[UIElement | list[UIElement]]`
+ src/bokeh/layouts.py:153:19: error[invalid-argument-type] Argument is incorrect: Expected `list[UIElement]`, found `list[UIElement | list[UIElement]]`
- Found 463 diagnostics
+ Found 467 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/expr/format.py:74:16: error[no-matching-overload] No overload of bound method `center` matches arguments
- Found 3287 diagnostics
+ Found 3286 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/sensor/recorder.py:613:27: error[invalid-assignment] Invalid assignment to key "max" with declared type `int | float` on TypedDict `StatisticData`: value of type `Unknown | islice[Unknown]`
- homeassistant/components/sensor/recorder.py:617:27: error[invalid-assignment] Invalid assignment to key "min" with declared type `int | float` on TypedDict `StatisticData`: value of type `Unknown | islice[Unknown]`
- Found 13740 diagnostics
+ Found 13738 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@AlexWaygood reviewed on 2025-09-22 12:59_

Nice! I think this approach still has some false negatives on "mixed tuples" with fixed-length suffixes. E.g. here's the current behaviour on this branch:

```py
def func(*args: str): ...

def _(
    a: tuple[str, ...],
    b: tuple[str, *tuple[str, ...]],
    c: tuple[str, *tuple[str, ...], str],
    d: tuple[int, *tuple[str, ...]],
    e: tuple[int, *tuple[str, ...], str],
    f: tuple[*tuple[str, ...], str],
    g: tuple[*tuple[str, ...], int],
    h: tuple[int, *tuple[str, ...], int],
):
    func(*a)  # no error (good)
    func(*b)  # no error (good)
    func(*c)  # no error (good)
    func(*d)  # error (good)
    func(*d)  # error (good)
    func(*e)  # error (good)
    func(*f)  # no error (good)
    func(*g)  # <-------------  no error (bad!)
    func(*h)  # error (good)
```

---

_Comment by @dhruvmanila on 2025-09-23 04:29_

I think the core issue here is that we've a way to structure a one-to-many mapping like `*args` argument to multiple parameters via the `MatchedArgument` but we don't have a way to structure a many-to-one mapping which will occur in this scenario where a variadic argument is being unpacked into a variadic parameter. I'm thinking if it's actually required or not, and if so how to structure it.

---

_Converted to draft by @dhruvmanila on 2025-09-23 14:07_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2505 on 2025-09-24 06:43_

The reason this isn't required now is because the unpacking of a variadic argument is being done during parameter matching itself. This was added in https://github.com/astral-sh/ruff/pull/20130 to make sure that specialization builder has access to the correct type for each parameter.

The `MatchedArgument` already stores the unpacked types for a variadic argument so we can directly use that here instead.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2145 on 2025-09-24 06:44_

The length isn't required here because we already have access to `Tuple<Type>` in this function so we can use that directly to get the `TupleLength`. This also simplifies the overload evaluation where we needed to change this length when argument type expansion lead to retrying of parameter matching.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2197 on 2025-09-24 06:46_

This is the core logic that would actually match the variadic argument with the variadic parameter in cases where it's missed today. Refer to mdtest examples.

---

_@dhruvmanila reviewed on 2025-09-24 06:46_

---

_Comment by @dhruvmanila on 2025-09-24 08:58_

## Ecosystem analysis

All of the new diagnostics in `ignite` are, I think, correct. The annotation of the variadic parameter for `register_events` function is `*event_names: Union[List[str], List[EventEnum]]` which suggests that each element of an unpacking should be a list but the provided parameter is unpacking the enum class directly.

```py
from enum import Enum

class Foo(Enum):
    foo = "foo"
    bar = "bar"

def f(*args: list[Foo]): ...

# This should raise a diagnostic because it unpacks into `Foo` variants but the
# annotated parameter type expects each element to be a `list[Foo]`
f(*Foo)
```

The diagnostic in `vision` is correct as well:
```py
import itertools
import string


def _(*digits: str):
    if not digits:
        digits = string.ascii_lowercase
    else:
        # ty: Object of type `str` is not assignable to `tuple[str, ...]` [invalid-assignment]
        digits = "".join(itertools.chain(*digits))
```

The diagnostic in `bokeh` seems to be related to generics which, if I remember correctly, was brought up during an early discussion regarding how to consider the entire union to map the concrete type so that it doesn't map it to `T` and instead considers `T | list[T]` (the details are a bit hazy in my mind right now).

```py
def f[T](*args: T | list[T]) -> list[T]:
    return []


class Foo: ...


def _(*args: Foo | list[Foo]):
	# This PR reveals this to be `list[Foo | list[Foo]]` and in `bokeh` it then creates a
    # diagnostic when it's being used where `list[Foo]` is expected.
    reveal_type(f(*args))
```

---

_Marked ready for review by @dhruvmanila on 2025-09-24 08:58_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-09-24 08:58_

---

_Comment by @dhruvmanila on 2025-09-24 08:59_

(This is something that I'd love to get @dcreager review on specifically as the original author for single-starred argument.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/function.md`:713 on 2025-09-24 14:28_

Could possibly consider adding some mixed-tuple cases here too (but not essential!)

```suggestion
def _(args1: list[int], args2: tuple[int], args3: tuple[int, int], args4: tuple[int, ...], args5: tuple[int, *tuple[str, ...]], args6: tuple[int, int, *tuple[str, ...]]) -> None:
    # error: [invalid-argument-type] "Argument to function `f` is incorrect: Expected `str`, found `int`"
    reveal_type(f(*args1))  # revealed: int

    # This shouldn't raise an error because the unpacking doesn't match the variadic parameter.
    reveal_type(f(*args2))  # revealed: int

    # But, this should because the second tuple element is not assignable.
    # error: [invalid-argument-type] "Argument to function `f` is incorrect: Expected `str`, found `int`"
    reveal_type(f(*args3))  # revealed: int

    # error: [invalid-argument-type] "Argument to function `f` is incorrect: Expected `str`, found `int`"
    reveal_type(f(*args4))  # revealed: int
    
    # The first element of the tuple matches the required argument;
    # all subsequent elements match the variadic argument
    reveal_type(f(*args5))  # revealed: int
    
    # error: [invalid-argument-type] "Argument to function `f` is incorrect: Expected `str`, found `int`"
    reveal_type(f(*args6))
```

---

_@AlexWaygood reviewed on 2025-09-24 14:30_

This looks reasonable to me, thanks! I agree it would be great to get @dcreager's eyes on it, though

---

_Review request for @carljm removed by @carljm on 2025-09-24 16:36_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:2151 on 2025-09-25 00:33_

It looks like you only ever use the variable element to extend the `all_elements` iterator with a never-ending stream of `variable_element` items. You could do the same thing by chaining a `std::iter::repeat` to the end of the `all_elements` iterator. Then you wouldn't need the `.or(variable_element)` calls below. (You might also consider adding that as an additional method on `Tuple` or `TupleSpec`)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:2145 on 2025-09-25 00:38_

That's a great catch!

---

_@dcreager approved on 2025-09-25 00:42_

This approach looks good! We still need to know the type of the splatted argument before we can perform parameter matching; otherwise we don't know how many elements are in the argument and therefore how many parameters it will be matched to. I think that's unavoidable, though. (And @ibraheemdev, I think this the main thing that might wreak havoc on your work to use bidirectional type checking with function arguments.)

---

_@dhruvmanila reviewed on 2025-09-25 07:44_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2151 on 2025-09-25 07:44_

That's a good idea although there's one place where I don't use the `.or(...)` call at the end which just loops over the remaining argument types if any. This is usually the case for suffix elements.

---

_Comment by @dhruvmanila on 2025-09-25 07:45_

> This approach looks good! We still need to know the type of the splatted argument before we can perform parameter matching; otherwise we don't know how many elements are in the argument and therefore how many parameters it will be matched to. I think that's unavoidable, though.

Yeah, I agree that this is unavoidable.

---

_@dhruvmanila reviewed on 2025-09-25 07:47_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/function.md`:713 on 2025-09-25 07:47_

Thanks, I've added them.

---

_Merged by @dhruvmanila on 2025-09-25 07:51_

---

_Closed by @dhruvmanila on 2025-09-25 07:51_

---

_Branch deleted on 2025-09-25 07:51_

---
