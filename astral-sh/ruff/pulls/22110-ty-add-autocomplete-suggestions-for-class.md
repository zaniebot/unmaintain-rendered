```yaml
number: 22110
title: "[ty] Add autocomplete suggestions for class arguments"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: add-class-parameter-completions
created_at: 2025-12-20T12:46:09Z
updated_at: 2026-01-15T18:26:49Z
url: https://github.com/astral-sh/ruff/pull/22110
synced_at: 2026-01-15T18:49:33Z
```

# [ty] Add autocomplete suggestions for class arguments

---

_@RasmusNygren_

## Summary

This adds autocomplete suggestions for class
arguments that we know are valid.
Specifically this adds the `metaclass` keyword
suggestion for all class definitions and
`typing.TypedDict` specific keywords whenever
a TypedDict class is constructed.

Fixes https://github.com/astral-sh/ty/issues/1886, fixes https://github.com/astral-sh/ty/issues/2264
Basically fixes https://github.com/astral-sh/ty/issues/1775, this currently only supports the `total` keyword, not the `closed` or `extra_items` keywords that are added in [PEP 728](https://peps.python.org/pep-0728/) (that doesn't land until 3.15).
Once we add support for PEP 728, I'm not sure if we only want to gate this on python >= 3.15 or if we should also consider if TypedDict is imported from `typing_extensions` and add special handling for that?


## Test Plan
New tests and validating in the playground.


---

_Review requested from @carljm by @RasmusNygren on 2025-12-20 12:46_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-12-20 12:46_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-12-20 12:46_

---

_Review requested from @dcreager by @RasmusNygren on 2025-12-20 12:46_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-12-20 12:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 12:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-20 12:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

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
- Found 5544 diagnostics
+ Found 5549 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1228:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5151 diagnostics
+ Found 5150 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14452 diagnostics
+ Found 14453 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:423 on 2025-12-20 12:49_

nit

```suggestion
        let documentation = documentation.clone().map(Docstring::new);
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1145 on 2025-12-20 12:51_

```suggestion
/// Some arguments we know are always valid and thus they are easy
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1162 on 2025-12-20 12:53_

While this is an accurate type, I think we can be more specific: the object passed to the `metaclass=` keyword not only needs to be an instance of `type`, it also needs to be a _subclass_ of `type` (it must inhabit the type `type[type]`):

```suggestion
        let ty = Some(KnownClass::Type.to_subclass_of(model.db()));
```

---

_@AlexWaygood reviewed on 2025-12-20 13:06_

Thank you, this looks excellent!!

Some more tests you could possibly add:

- `metaclass=` keyword completion for a generic class:

  ```py
  class Foo[T](meta<CURSOR>)
  ```

- `total=` keyword completion for a class that is a `TypedDict` because it has `TypedDict` somewhere in its MRO (but does not directly inherit from `TypedDict`):

  ```py
  from typing import TypedDict

  class Foo(TypedDict):
      x: int

  class Bar(Foo, to<CURSOR>)
  ```

- `total=` keyword completion for new-style generic `TypedDict`s:

  ```py
  from typing import TypedDict

  class Foo[T](TypedDict, to<CURSOR>)
  ```

- `total=` keyword completions for old-style generic TypedDicts:

  ```py
  from typing import Generic, TypeVar, TypedDict

  T = TypeVar("T")

  class Foo(TypedDict, Generic[T], to<CURSOR>)
  ```

---

_Label `server` added by @AlexWaygood on 2025-12-20 13:07_

---

_Label `ty` added by @AlexWaygood on 2025-12-20 13:07_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-12-20 13:16_

---

_@RasmusNygren reviewed on 2025-12-20 13:41_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:423 on 2025-12-20 13:41_

`Docstring::new` expects String specifically so I don't think that would work unless the function signature is changed as well.

---

_@RasmusNygren reviewed on 2025-12-20 13:49_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:1162 on 2025-12-20 13:49_

Yep that makes a lot of sense, fixed!

---

_Comment by @RasmusNygren on 2025-12-20 13:52_

> Thank you, this looks excellent!!
> 
> Some more tests you could possibly add:
> 
>     * `metaclass=` keyword completion for a generic class:
>       ```python
>       class Foo[T](meta<CURSOR>)
>       ```
> 
>     * `total=` keyword completion for a class that is a `TypedDict` because it has `TypedDict` somewhere in its MRO (but does not directly inherit from `TypedDict`):
>       ```python
>       from typing import TypedDict
>       
>       class Foo(TypedDict):
>           x: int
>       
>       class Bar(Foo, to<CURSOR>)
>       ```
> 
>     * `total=` keyword completion for new-style generic `TypedDict`s:
>       ```python
>       from typing import TypedDict
>       
>       class Foo[T](TypedDict, to<CURSOR>)
>       ```
> 
>     * `total=` keyword completions for old-style generic TypedDicts:
>       ```python
>       from typing import Generic, TypeVar, TypedDict
>       
>       T = TypeVar("T")
>       
>       class Foo(TypedDict, Generic[T], to<CURSOR>)
>       ```

Very reasonable, and thank you for the examples, I just added them as is :)

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:423 on 2025-12-20 13:53_

Ah right, yeah, my bad

---

_@AlexWaygood reviewed on 2025-12-20 13:53_

---

_@AlexWaygood approved on 2025-12-20 13:56_

LGTM (but will wait until @BurntSushi's had a chance to take a look).

>  Once we add support for PEP 728, I'm not sure if we only want to gate this on python >= 3.15 or if we should also consider if TypedDict is imported from typing_extensions and add special handling for that?

Yeah, our `SpecialFormType::TypedDict` variant currently covers both `typing.TypedDict` and `typing_extensions.TypedDict`. We'll probably want to split it into two variants when we add support for PEP 728, so we can distinguish between classes that inherit from `typing_extensions.TypedDict` (and can therefore use PEP 728 on older versions), and those that inherit from `typing.TypedDict` (and can therefore only use PEP 728 on very new versions of Python).

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:78 on 2025-12-22 09:48_

I don't think it really makes sense here to pass `db`, `file` and `model` because `model` is just a thin wrapper around `db` + `file`. If we need model, then I suggest removing `db` and `file`.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:1091 on 2025-12-22 09:58_

I wonder if we should store the `CoveringNode` in `ScopedTarget` target to avoid re-traversing the AST multiple times to get the covering node

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:1138 on 2025-12-22 10:01_

I think we should always exit after reaching a call or class def statement
```suggestion
            ast::AnyNodeRef::ExprCall(call)=>
            {
                if call.arguments.range().contains_range(cursor.range) {
	                add_function_arg_completions(db, file, cursor, completions);
                }
                return;
            }
            ast::AnyNodeRef::StmtClassDef(class_def) {
            	if let Some(arguments) = class_def.arguments.as_ref() &&
            		arguments.range().contains_range(cursor.range)) {
	                add_class_arg_completions(model, class_def, completions);
            		}
  
                return;
            }
            node => {
            	if node.is_statement() {
            		return;
          		}
            }
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:1125 on 2025-12-22 10:03_

Would it make sense to pass `arguments` here to avoid having to handle the case where `class_def.arguments` is `None`?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:448 on 2025-12-22 10:04_

This feels slightly too specific to me to have in `ide_support`. I'd inline this function

---

_@MichaReiser reviewed on 2025-12-22 10:04_

---

_@RasmusNygren reviewed on 2025-12-22 12:44_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:78 on 2025-12-22 12:44_

While I do agree, I'm not sure it's that small of a change because I believe that means that `add_function_arg_completions` would have to take a `ty_python_semantic::Db` causing type issues down the call-chain.

---

_@RasmusNygren reviewed on 2025-12-22 12:46_

---

_Review comment by @RasmusNygren on `crates/ty_python_semantic/src/types/ide_support.rs`:448 on 2025-12-22 12:46_

Ye, that's fair, it does mean that we need to raise the visibility of `is_typed_dict` but I guess that's fine

---

_@RasmusNygren reviewed on 2025-12-22 12:51_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:1125 on 2025-12-22 12:51_

It probably makes sense. In some way however I think I like that you explicitly have to pass the class_def as it sort of self-documents that you must already have identified a StmtClassDef node before calling the function, but maybe that's dumb reasoning :D

---

_@RasmusNygren reviewed on 2025-12-22 12:53_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:1138 on 2025-12-22 12:53_

Yeah, that's a very fair point

---

_@RasmusNygren reviewed on 2025-12-22 13:08_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:1091 on 2025-12-22 13:08_

I don't necessarily think that we need to fix that right now but at some point I think that is probably the way to go as it's already being called in a few places. It might not even be insane to store the `CoveringNode` directly on the context?

---

_@MichaReiser reviewed on 2025-12-22 17:18_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:78 on 2025-12-22 17:18_

Hmm yeah, this is a bit annoying. We could stop passing `file` and retrieved that from `model` 

---

_@RasmusNygren reviewed on 2025-12-22 21:19_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:78 on 2025-12-22 21:19_

Yep I agree, that's what I went for in the end.

---

_Review request for @carljm removed by @carljm on 2025-12-23 01:48_

---

_Comment by @AlexWaygood on 2025-12-30 13:10_

I'm going to land this because I keep finding issues where this PR fixes the behaviour 😆 I literally have another issue written up in another tab right now, and my mouse is ready to hit the "submit" button... but there's no point, because this PR seems to fix it 😆

I'm sure @BurntSushi won't mind posting post-merge review comments if there's anything he spots when he's back from his Christmas break!

---

_Merged by @AlexWaygood on 2025-12-30 13:10_

---

_Closed by @AlexWaygood on 2025-12-30 13:10_

---

_@BurntSushi reviewed on 2026-01-06 14:36_

This looks great to me! Awesome work!

---

_Branch deleted on 2026-01-15 18:26_

---
