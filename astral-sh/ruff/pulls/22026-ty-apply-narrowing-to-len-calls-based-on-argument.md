```yaml
number: 22026
title: "[ty] Apply narrowing to `len` calls based on argument size"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/len
created_at: 2025-12-17T14:02:00Z
updated_at: 2025-12-17T18:16:00Z
url: https://github.com/astral-sh/ruff/pull/22026
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Apply narrowing to `len` calls based on argument size

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/1983.


---

_Comment by @astral-sh-bot[bot] on 2025-12-17 14:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 14:05_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2792 diagnostics
+ Found 2789 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2082 diagnostics
+ Found 2080 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_groupby.py:439:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
+ tests/test_groupby.py:439:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
- tests/test_resampler.py:402:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
+ tests/test_resampler.py:402:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14392 diagnostics
+ Found 14391 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/stats/_continuous_distns.py:2853:45: error[unsupported-operator] Operator `/` is not supported between objects of type `Literal[2]` and `None | Unknown`
+ scipy/stats/_continuous_distns.py:2853:45: error[unsupported-operator] Operator `/` is not supported between objects of type `Literal[2]` and `None | @Todo`
- scipy/stats/_continuous_distns.py:2853:63: error[unsupported-operator] Operator `/` is not supported between objects of type `Literal[1]` and `None | Unknown`
+ scipy/stats/_continuous_distns.py:2853:63: error[unsupported-operator] Operator `/` is not supported between objects of type `Literal[1]` and `None | @Todo`
- scipy/stats/_continuous_distns.py:2859:42: error[unsupported-operator] Operator `/` is not supported between objects of type `Literal[1]` and `None | Unknown`
+ scipy/stats/_continuous_distns.py:2859:42: error[unsupported-operator] Operator `/` is not supported between objects of type `Literal[1]` and `None | @Todo`
- scipy/stats/_continuous_distns.py:9883:33: error[unsupported-operator] Operator `**` is not supported between objects of type `Unknown | None` and `Literal[2]`
+ scipy/stats/_continuous_distns.py:9883:33: error[unsupported-operator] Operator `**` is not supported between objects of type `@Todo | None` and `Literal[2]`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @ntBre on 2025-12-17 14:06_

---

_Renamed from "Apply narrowing to `len` calls based on argument size" to "[ty] Apply narrowing to `len` calls based on argument size" by @charliermarsh on 2025-12-17 14:06_

---

_Marked ready for review by @charliermarsh on 2025-12-17 14:10_

---

_Review requested from @carljm by @charliermarsh on 2025-12-17 14:10_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-17 14:10_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-17 14:10_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-17 14:10_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-17 14:14_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 14:21_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 2 | 45 |
| `invalid-return-type` | 0 | 2 | 3 |
| `unsupported-operator` | 0 | 1 | 4 |
| `unused-ignore-comment` | 4 | 0 | 0 |
| `possibly-missing-attribute` | 0 | 0 | 2 |
| `type-assertion-failure` | 0 | 0 | 2 |
| `non-subscriptable` | 0 | 1 | 0 |
| **Total** | **4** | **6** | **56** |

**[Full report with detailed diff](https://charlie-len.ecosystem-663.pages.dev/diff)** ([timing results](https://charlie-len.ecosystem-663.pages.dev/timing))




---

_@AlexWaygood reviewed on 2025-12-17 14:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 14:22_

this introduces an unsoundness: it's possible that a user class could have a `__bool__` method that "disagrees" with its `__len__` method, which would mean that we would apply incorrect narrowing here. For example:

```py
from typing import Literal

class Tricksy:
    def __bool__(self) -> Literal[True]:
        return True

    def __len__(self):
        return 0

def f(x: Tricksy | Literal[""]):
    if len(x):
        reveal_type(x)  # `Tricksy` on your PR, but unreachable at runtime
    else:
        reveal_type(x)  # `Literal[""]` on your PR, but reachable at runtime
                        # even if an instance of `Tricksy` was passed in
```

---

_@AlexWaygood reviewed on 2025-12-17 14:25_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 14:25_

To address the unsoundness, I think we should _only_ apply this narrowing if the pre-existing type is one of the following types, or a union of the following types. We know that there can be no subtypes of these types, and we know that their `__bool__`/`__len__` methods are well-behaved:
- String-literal types
- Bytes-literal types
- `LiteralString`
- nominal-instance types with tuple specs.

  These _can_ be subtyped, but we already have plans to emit a diagnostic if a user creates a subclass of `tuple` where the `__bool__` method "disagrees" with the `__len__` method.

---

_@charliermarsh reviewed on 2025-12-17 14:29_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 14:29_

Are there any examples elsewhere of how we handle this? (Is it a blocking concern?)

---

_@AlexWaygood reviewed on 2025-12-17 14:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 14:32_

> Is it a blocking concern?

I think so? We've tried very hard to respect this kind of soundness in our narrowing so far

---

_@AlexWaygood reviewed on 2025-12-17 14:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 14:33_

> Are there any examples elsewhere of how we handle this?

we do something similar for `==` narrowing here: https://github.com/astral-sh/ruff/blob/421f88bb327c8d4685a5e5cf4ca92533d30ef698/crates/ty_python_semantic/src/types/narrow.rs#L488-L603

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 14:35_

(Sorry I wrote my comment before seeing your reply. Thank you!)

---

_@charliermarsh reviewed on 2025-12-17 14:35_

---

_@charliermarsh reviewed on 2025-12-17 14:45_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 14:45_

So does that mean we can't narrow `str` or `list`?

---

_@AlexWaygood reviewed on 2025-12-17 15:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 15:01_

I think so -- you might have a user subclass of either of those that overrides `__len__` or `__bool__` in a naughty way.

Like, it might be that we're being way too pedantic about this at the moment. I think that's a very valid concern. There are some issues open asking if our current behaviour for `==` narrowing is way too pedantic, because we currently only narrow in situations where we can prove it's sound (where we can prove it's impossible to have a subtype with a naughty `__eq__` or `__ne__` method).

But I think our narrowing here should, at the moment, be consistent with our `==` narrowing. If we want to change our behaviour for this kind of thing, we should change it all together for our various forms of narrowing. Inconsistent behaviour for this kind of thing would be really confusing

---

_@charliermarsh reviewed on 2025-12-17 15:28_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 15:28_

In the case from the linked issue though:
```python
lines: list[str] = ["foo", "bar", "baz"]
for line in lines:
    if not line:
        continue

    value = line if len(line) < 3 else ""
    if len(value) and value[0] == " ":
        value = value[1:]
```

Am I right that we actually _could_ support this (even ignoring the fact that it's defined with string literals) because we have `if not line:` within the loop? So we actually _do_ know that `__bool__` will not return `False` in this case?

---

_@AlexWaygood reviewed on 2025-12-17 15:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 15:35_

you have to keep track of exactly how many items are in the list in order to know whether `bool(x)` is going to continue returning `True` or `False` in a consistent way across a function, if `x` has type `list[str]`. If the list becomes empty, it's now falsy. That's quite a hard analysis to pull off.

---

_@charliermarsh reviewed on 2025-12-17 15:43_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 15:43_

I'm referring to `line` here, not `lines`.

---

_@AlexWaygood reviewed on 2025-12-17 15:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 15:45_

ah, sorry. But an object of type `list[str]` could have mutable user subclasses of `str` in it, and those user subclasses of `str` could have `__bool__` methods that are inconsistent about their truthiness in a similar way to how `list` is...

---

_@charliermarsh reviewed on 2025-12-17 15:48_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/narrow.rs`:912 on 2025-12-17 15:48_

But by the time we get to `value = value[1:]`, we have checked _both_ `if line` and `len(line)`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/len.md`:63 on 2025-12-17 17:31_

you could _possibly_ add TODOs here that ideally we'd narrow these types further, e.g. to

```suggestion
    if len(x):
        reveal_type(x)  # revealed: tuple[int, ...] & ~tuple[()]
    else:
        reveal_type(x)  # revealed: tuple[()]
```

(which is related to https://github.com/astral-sh/ty/issues/560)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/len.md`:91 on 2025-12-17 17:35_

and even if it couldn't: the `__bool__` of `list` can anyway give inconsistent results throughout the course of a function. E.g. if you have this, say we know for sure that `x` is _exactly_ `list[int]`, not a subclass thereof:

```py
def f(x: list[int]):
    if len(x):
        reveal_type(x)  # say we infer it as `list[int] & AlwaysTruthy` now
        x.clear()
        reveal_type(x)  # what's the type now? Still `list[int] & AlwaysTruthy`?
                        # how do we detect that the `.clear()` call invalidates the previous narrowing?

        do_something_with_x(x)  # does _this_ function call invalidate the narrowing?
                                # it's a black box to us: we don't know what happens inside it
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/len.md`:121 on 2025-12-17 17:36_

you could assert these with `reveal_type` calls, to make sure that the comments don't go out of date

```suggestion
        reveal_type(line) # revealed: str & ~AlwaysFalsy
        value = line if len(line) < 3 else ""
        reveal_type(value)  # revealed: (str & ~AlwaysFalsy) | Literal[""]
```

---

_@AlexWaygood approved on 2025-12-17 17:43_

Thank you. This looks great

---

_Merged by @charliermarsh on 2025-12-17 18:15_

---

_Closed by @charliermarsh on 2025-12-17 18:15_

---

_Branch deleted on 2025-12-17 18:16_

---
