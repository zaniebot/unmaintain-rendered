```yaml
number: 22648
title: "[ty] Set `None` as the `definition` of `Self` type variables"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/typevar-definition
created_at: 2026-01-17T12:50:14Z
updated_at: 2026-01-18T19:17:35Z
url: https://github.com/astral-sh/ruff/pull/22648
synced_at: 2026-01-18T20:18:40Z
```

# [ty] Set `None` as the `definition` of `Self` type variables

---

_@AlexWaygood_

## Summary

https://github.com/astral-sh/ty/issues/2514 was fixed in https://github.com/astral-sh/ruff/pull/22646, but the underlying cause of the bug was that the `definition` field of a `Self` typevar currently points to the `Definition` of the typevar's containing class. That's a bug magnet, because the `Definition` range of a class will often be far larger than the normal `Definition` range of a TypeVar.

This PR makes such bugs impossible by setting `None` as the `definition` of `Self` type variables.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @AlexWaygood on 2026-01-17 12:50_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-17 12:50_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-17 12:50_

---

_Label `internal` added by @AlexWaygood on 2026-01-17 12:50_

---

_Label `ty` added by @AlexWaygood on 2026-01-17 12:50_

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 12:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-17 12:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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
- Found 5406 diagnostics
+ Found 5411 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14496 diagnostics
+ Found 14497 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~12MB
+     struct fields = ~11MB


```

</details>




---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-17 13:52_

How does this affect the LSP? Specifically, go to type definition, hover, and rename?

Could #22646 instead have used `focus_range` instead of `full_range`?

---

_@MichaReiser reviewed on 2026-01-17 13:54_

---

_@AlexWaygood reviewed on 2026-01-17 13:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-17 13:56_

Currently, goto-definition on a `Self` type variable will take you to the definition of the enclosing class, which I don't think is what the user would want because `Self` is not defined there. I think this improves the experience for LSP users.

---

_@AlexWaygood reviewed on 2026-01-17 13:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-17 13:57_

For example:

```py
from typing import Self

class Foo:

    # <-- 500 lines or so of code here... -->

    def f(self, other: Self) -> None: ...
                     # ^^^^
```

I don't think anybody would expect goto-definition on the `Self` type variable there to take them to the definition of `Foo`

---

_@MichaReiser reviewed on 2026-01-17 14:00_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-17 14:00_

I would expect go-to-type definition to go to `Self`. I'm not sure what to expect from `Hover` or go to definition. What does find-references return? Does it return all references to `Self` in the file?

---

_@AlexWaygood reviewed on 2026-01-17 14:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-17 14:04_

> I would expect go-to-type definition to go to `Self`.

As in, the definition of the `Self` special form in typeshed's `typing.pyi` stub? We could do that. It has some useful documentation about what `typing.Self` is and represents.

It'll retain the bug magnet for subdiagnostics, though. We still wouldn't ever want a subdiagnostic to point to the `Self: _SpecialForm` definition in `typing.pyi` when we're trying to tell a user where their type variable was defined. It doesn't give any helpful information about the upper bound or default of the type variable (because the upper bound of `Self` depends on the context of the class it's being used in, unlike any other type variable).

---

_@MichaReiser reviewed on 2026-01-17 14:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-17 14:13_

I've found it useful in the past that `r-a` allows me to jump to the class definition when clicking on `Self`. It also shows you the docs of the class when hovering `Self` which, to me, also makes sense, because that's what `Self` resolves to. 

`Self` is inherently awkward in the LSP case because its a bound type var and using it in a method binds it. 

I'm leaning towards not changing this, as the new behavior isn't clearly an improvement. If we decide that jumping nowhere is the desired behavior, then we should add LSP tests for all features that demonstrate that (e.g. does this break semantic tokens?)

---

_@AlexWaygood reviewed on 2026-01-18 18:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-18 18:34_

I agree that we definitely need to add language-server snapshots for our behaviour around `Self`; I'm surprised that none needed to be updated as a result of this PR.

I still think that I wouldn't personally expect cmd-clicking on `Self` to take me to the class definition in an IDE, but I see your point that it might be nice for documentation about the class (rather than documentation about `Self` types in general) to show up when you hover over `Self` in the context of a method annotation. (There is a cost to that, though -- it means that users will rarely see [the documentation for the `Self` type itself](https://github.com/astral-sh/ruff/blob/bab571c12c46dd1a8b3070df55b52b718832e17c/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi#L566-L581) that's in `typing.pyi`.)

But none of this solves the original issue that I was trying to solve, which is that if you're attaching a subdiagnostic that points to the definition of a type variable (so that the user can see what the bounds, constraints or defaults of that variable are), you never really want to attach that subdiagnostic if the type variable happens to be `Self`. Whether we call `.full_range()` or `.focus_range()` on the definition is irrelevant; for diagnostics like the one being highlighted in https://github.com/astral-sh/ty/issues/2514, we just don't want to add a subdiagnostic highlighting the class definition at all, because it's not helpful information for the user in that context. Even if we stipulate that for some language-server purposes, it's useful for the `definition` field of `Self` type variables to point to the class's definition, it's still a very different kind of definition to the kind of definition that most type variables have, and that's a footgun for us in `ty_python_semantic` when we're creating subdiagnostics.

If we want go-to definition, on-hover, etc. to treat `Self` as if it's the class, then I wonder if it might be cleaner for that to be implemented as a special case in the `ty_ide` crate rather than having the `definition` field here point to the class definition. (I'm happy to implement that.) Another strategy might be to have a separate `definition_for_subdiagnostics()` method that we could use from `ty_python_semantic`, but I don't really feel like this would get rid of the footgun because we'd still have to remember to use it whenever we create subdiagnostics.

---

_@MichaReiser reviewed on 2026-01-18 18:55_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-18 18:55_

> If we want go-to definition, on-hover, etc. to treat Self as if it's the class, then I wonder if it might be cleaner for that to be implemented as a special case in the ty_ide crate rather than having the definition field here point to the class definition.

I'm not sure but this might require special casing in multiple places. 

Ultimately, I feel like it's important do decide what we expect `Type::definition` to return to decide on what the right behavior here is, because this is very much an API that could also be used by a linter in the future. 

In that context, I feel like there's no right answer:

* Returning the class feels wrong because it's not where the `TypeVar`'s defined
* Returning `typing.Self` feels correct as it's where the symbol's defined. But it's not how the type var is bound.

I guess returning `None` is fine for now, but let's create an issue to follow up on what behavior we want (and add tests)




---

_@MichaReiser approved on 2026-01-18 18:55_

---

_@AlexWaygood reviewed on 2026-01-18 19:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-18 19:03_

> I guess returning `None` is fine for now, but let's create an issue to follow up on what behavior we want (and add tests)

Sounds good.

---

_Merged by @AlexWaygood on 2026-01-18 19:04_

---

_Closed by @AlexWaygood on 2026-01-18 19:04_

---

_Branch deleted on 2026-01-18 19:04_

---

_@AlexWaygood reviewed on 2026-01-18 19:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:106 on 2026-01-18 19:17_

https://github.com/astral-sh/ty/issues/2557 is the followup issue

---
