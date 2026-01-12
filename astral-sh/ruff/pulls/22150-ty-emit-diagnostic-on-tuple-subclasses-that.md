```yaml
number: 22150
title: "[ty] Emit diagnostic on tuple subclasses that override `__eq__`, `__ne__` or `__bool__` methods"
type: pull_request
state: open
author: MatthewMckee4
labels:
  - ty
assignees: []
base: main
head: tuple-subclass-dunder-override
created_at: 2025-12-22T23:50:02Z
updated_at: 2026-01-12T18:29:45Z
url: https://github.com/astral-sh/ruff/pull/22150
synced_at: 2026-01-12T19:26:23Z
```

# [ty] Emit diagnostic on tuple subclasses that override `__eq__`, `__ne__` or `__bool__` methods

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/2171

## Test Plan

Add `unsafe_tuple_subclass.md` mdtest file.


---

_Comment by @astral-sh-bot[bot] on 2025-12-22 23:52_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-22 23:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
+ parso/utils.py:151:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
+ parso/utils.py:158:9: warning[unsafe-tuple-subclass] Unsafe override of method `__ne__` in a subclass of `tuple`
- Found 199 diagnostics
+ Found 201 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/rich/segment.py:100:9: warning[unsafe-tuple-subclass] Unsafe override of method `__bool__` in a subclass of `tuple`
+ src/pip/_vendor/rich/text.py:60:9: warning[unsafe-tuple-subclass] Unsafe override of method `__bool__` in a subclass of `tuple`
- Found 603 diagnostics
+ Found 605 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/structs/diffs.py:53:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
+ kopf/_cogs/structs/diffs.py:59:9: warning[unsafe-tuple-subclass] Unsafe override of method `__ne__` in a subclass of `tuple`
- Found 267 diagnostics
+ Found 269 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/language/location.py:31:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
+ src/graphql/language/location.py:36:9: warning[unsafe-tuple-subclass] Unsafe override of method `__ne__` in a subclass of `tuple`
- Found 640 diagnostics
+ Found 642 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/segment.py:100:9: warning[unsafe-tuple-subclass] Unsafe override of method `__bool__` in a subclass of `tuple`
+ rich/text.py:60:9: warning[unsafe-tuple-subclass] Unsafe override of method `__bool__` in a subclass of `tuple`
- Found 345 diagnostics
+ Found 347 diagnostics

pylint (https://github.com/pycqa/pylint)
+ pylint/checkers/symilar.py:186:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
- Found 216 diagnostics
+ Found 217 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
+ dedupe/predicates.py:356:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
- Found 52 diagnostics
+ Found 53 diagnostics

mypy (https://github.com/python/mypy)
+ mypy/typeshed/stdlib/importlib/metadata/__init__.pyi:89:13: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
+ mypy/typeshed/stdlib/unittest/mock.pyi:90:13: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
+ mypy/typeshed/stdlib/unittest/mock.pyi:91:13: warning[unsafe-tuple-subclass] Unsafe override of method `__ne__` in a subclass of `tuple`
+ mypy/typeshed/stdlib/unittest/mock.pyi:121:13: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
+ mypy/typeshed/stdlib/unittest/mock.pyi:122:13: warning[unsafe-tuple-subclass] Unsafe override of method `__ne__` in a subclass of `tuple`
- Found 1740 diagnostics
+ Found 1745 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ pymongo/message.py:1894:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
+ pymongo/message.py:1899:9: warning[unsafe-tuple-subclass] Unsafe override of method `__ne__` in a subclass of `tuple`
- Found 447 diagnostics
+ Found 449 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/app_commands/namespace.py:53:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
- Found 548 diagnostics
+ Found 549 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/utilities/annotations.py:33:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
- Found 5369 diagnostics
+ Found 5375 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/pjit.py:128:7: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
- Found 2805 diagnostics
+ Found 2806 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1827 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/combinatorics/free_groups.py:706:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
- Found 15597 diagnostics
+ Found 15598 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/bitcoin/xpub.py:53:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/types.py:399:9: warning[unsafe-tuple-subclass] Unsafe override of method `__eq__` in a subclass of `tuple`
- Found 2102 diagnostics
+ Found 2105 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5168 diagnostics
+ Found 5170 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14505 diagnostics
+ Found 14504 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MatthewMckee4 reviewed on 2025-12-23 00:17_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/diagnostic.rs`:4074 on 2025-12-23 00:17_

I'm not sure how much we should explain *why* it is unsafe to override certain methods.

---

_Label `ty` added by @ntBre on 2025-12-23 00:20_

---

_Renamed from "Emit diagnostic on tuple subclasses that override certain dunder methods" to "[ty] Warn on tuple subclasses that override certain dunder methods" by @MatthewMckee4 on 2025-12-23 00:27_

---

_Marked ready for review by @MatthewMckee4 on 2025-12-23 09:45_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-12-23 09:45_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-12-23 09:45_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-12-23 09:45_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-12-23 09:45_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-12-23 09:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/boolean.md`:105 on 2025-12-23 10:38_

I don't think we need to complain about this one. I think we only need to complain about `__bool__` or `__len__` overrides if there's a possibility that this would violate our assumptions elsewhere.

This class is a subclass of a tuple with an unknown length, so we wouldn't ever make any assumptions about whether it's truthy or falsy anyway. So there's no unsoundness here, and there's no need for us to issue a diagnostic.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/len.md`:101 on 2025-12-23 10:39_

same here, I don't think we need to complain about this override. We wouldn't make any assumptions about the length or the truthiness of a `tuple[int, ...]` subtype, so this override is safe

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/len.md`:107 on 2025-12-23 10:40_

I don't think we need to issue an `unsafe-tuple-subclass` diagnostic here if we'll naturally emit a Liskov diagnostic anyway. The two diagnostics are warning about essentially the same problem

---

_@AlexWaygood reviewed on 2025-12-23 10:48_

Thanks!

This looks like it's a _bit_ too trigger-happy right now:
- I don't think we ever need to issue `unsafe-tuple-subclass` for a `__len__` override. Our existing Liskov Substitution Principle check takes care of that for us
- For other methods such as `__bool__`, `__eq__`, etc., I think we should avoid emitting this lint if the override would anyway cause us to emit a Liskov diagnostic. For something like this, we don't yet emit a Liskov diagnostic, but [we should](https://github.com/astral-sh/ty/issues/2156), so I don't think it should be covered by this check:
  ```py
  class Foo(tuple[int]):
      __len__ = None
  ```
- I don't think we need to issue `unsafe-tuple-subclass` for a `__bool__` override unless we would infer the tuple superclass as either always-truthy or always-falsy. It's fine to override `__bool__` on a tuple subclass if the superclass has ambiguous truthiness; that won't violate any of our type-inference or narrowing assumptions elsewhere

---

_@AlexWaygood reviewed on 2025-12-23 10:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:461 on 2025-12-23 10:58_

other methods where we have special-cased behaviour for tuples are `__lt__`, `__le__`, `__gt__`, `__ge__`, `__ne__`, and `__contains__`. We probably need to ban overriding all of those too, unfortunately.

But as I said elsewhere, I think we can get rid of `__len__` from this list (it's already covered by our Liskov checks). And there are many situations where overriding `__bool__` on a tuple subclass should be fine.

We have very special-cased behaviour for `__getitem__`, but that should also be taken care of by our Liskov checks once we've finished implementing them, so I don't think you need to worry about `__getitem__`.

---

_@AlexWaygood reviewed on 2025-12-23 12:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:461 on 2025-12-23 12:20_

> We have very special-cased behaviour for `__getitem__`, but that should also be taken care of by our Liskov checks once we've finished implementing them, so I don't think you need to worry about `__getitem__`.

Ah, no, we do need to ban `__getitem__` overrides too, I think, because our special casing for slices of tuples is not reflected in our synthesized `__getitem__` overloads for specialized tuples.

And there is maybe one _specific_ kind of `__len__` override that we need to ban, which is `__len__` being overridden on subclasses of "mixed" tuples. `tuple[int, *tuple[int, ...]]` means "a tuple of length at least one, but of potentially arbitrary length". Either of these overrides would not be caught by our Liskov checks, but both are clearly inconsistent with the tuple spec of the superclass:

```py
from typing import Literal

class Foo(tuple[int, *tuple[int, ...]]):
    def __len__(self) -> int:
        return 2

class Bar(tuple[int, *tuple[int, ...]):
    def __len__(self) -> Literal[2]:
        return 2
```

---

_@MatthewMckee4 reviewed on 2025-12-23 15:48_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/unsafe_tuple_subclass.md`:48 on 2025-12-23 15:48_

I assume this is okay as we shouldn't complain about unannotated methods?

---

_@AlexWaygood reviewed on 2025-12-23 15:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/unsafe_tuple_subclass.md`:48 on 2025-12-23 15:58_

Yeah, I think that's fine. We also don't issue any diagnostics for Liskov violations that could come about as a result of an override where the subclass method isn't annotated. This is consistent with that behaviour

---

_@MatthewMckee4 reviewed on 2025-12-23 16:00_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/unsafe_tuple_subclass.md`:48 on 2025-12-23 16:00_

Okay thanks

---

_@MatthewMckee4 reviewed on 2025-12-23 16:00_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/unsafe_tuple_subclass.md`:63 on 2025-12-23 16:00_

Should we emit a diagnostic here?

---

_@AlexWaygood reviewed on 2025-12-23 16:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/unsafe_tuple_subclass.md`:63 on 2025-12-23 16:03_

Yeah. We'd infer the superclass as `AlwaysFalsy`, but the subclass override obscures that. If it was annotated as returning `Literal[False]` that would be fine, but `bool` isn't consistent with an always-falsy tuple

---

_Comment by @MatthewMckee4 on 2025-12-23 16:04_

Perhaps this can be repurposed to just cover `__eq__`, `__ne__` and `__bool__`

---

_Comment by @AlexWaygood on 2025-12-23 16:06_

> Perhaps this can be repurposed to just cover `__eq__`, `__ne__` and `__bool__`

Sure!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/unsafe_tuple_subclass.md`:1 on 2025-12-23 16:12_

Can you add a few tests for overrides of "mixed" tuples?

```py
class A(tuple[int, *tuple[int, ...]]):
    def __bool__(self): ...  # fine

class B(tuple[int, *tuple[int, ...]]):
    def __bool__(self) -> Literal[True]: ...  # fine

class C(tuple[int, *tuple[int, ...]]):
    def __bool__(self) -> bool: ...  # error

class D(tuple[int, *tuple[int, ...]]):
    def __bool__(self) -> Literal[False]: ...  # error

class A(tuple[*tuple[int, ...], str]):
    def __bool__(self): ...  # fine

class B(tuple[*tuple[int, ...], str]):
    def __bool__(self) -> Literal[True]: ...  # fine

class C(tuple[*tuple[int, ...], str]):
    def __bool__(self) -> bool: ...  # error

class D(tuple[*tuple[int, ...], str]):
    def __bool__(self) -> Literal[False]: ...  # error
```

You'll need to add a block like this to those tests or you'll get spurious `invalid-syntax` errors issued:

````md
```toml
[environment]
python-version = "3.11"
```
````

---

_Converted to draft by @MatthewMckee4 on 2025-12-23 16:21_

---

_Marked ready for review by @MatthewMckee4 on 2025-12-23 16:22_

---

_Comment by @MatthewMckee4 on 2025-12-23 16:23_

I maybe need some advice on diagnostic messages

---

_Renamed from "[ty] Warn on tuple subclasses that override certain dunder methods" to "[ty] Emit diagnostic on tuple subclasses that override `__eq__`, `__ne__` or `__bool__` methods" by @MatthewMckee4 on 2025-12-23 16:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:418 on 2025-12-23 16:27_

we already have a `ClassBase::into_class()` method that does this, so I don't think this is necessary

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:3866 on 2025-12-23 16:28_

should this be a method on `FunctionType`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:587 on 2025-12-23 16:28_

this is a fantastic subdiagnostic, thank you!

---

_@AlexWaygood reviewed on 2025-12-23 16:28_

---

_Review request for @carljm removed by @carljm on 2025-12-24 01:18_

---

_Comment by @MichaReiser on 2025-12-29 09:37_

Thank you for your work on this feature. Almost the entire team is out this week. It may take a few days before someone finds time to review your PR. Happy holidays.

---
