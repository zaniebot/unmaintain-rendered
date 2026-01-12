```yaml
number: 22346
title: "[ty] Remove `ClassBase::TypedDict`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: main
head: charlie/typed-mro
created_at: 2026-01-02T20:19:58Z
updated_at: 2026-01-10T18:53:58Z
url: https://github.com/astral-sh/ruff/pull/22346
synced_at: 2026-01-12T15:57:47Z
```

# [ty] Remove `ClassBase::TypedDict`

---

_@charliermarsh_

## Summary

This is likely present for a reason that I don't understand, but this removes some special-casing, better models runtime behavior (`dict` in the MRO), and enables proper override checking. For reasons that (unfortunately) won't be clear in this PR, I believe this will also make functional TypedDict support a little simpler, since we can avoid extra TypedDict special-cases in `ClassMemberResult` and `InstanceMemberResult`.


---

_Label `ty` added by @charliermarsh on 2026-01-02 20:20_

---

_Comment by @astral-sh-bot[bot] on 2026-01-02 20:21_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-02 20:26_


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

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

discord.py (https://github.com/Rapptz/discord.py)
+ discord/permissions.py:40:5: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_BasePermissionsKwargs` with bases list `[<special-form 'typing.Generic[BoolOrNoneT]'>, <special-form 'typing.TypedDict'>]`
- Found 552 diagnostics
+ Found 553 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14473 diagnostics
+ Found 14474 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2026-01-02 20:37_

The `discord.py` diagnostic is concerning, looking into it.

---

_Comment by @AlexWaygood on 2026-01-02 20:45_

> The `discord.py` diagnostic is concerning, looking into it.

It's probably an issue due to typeshed telling us that `dict` has `Generic` in its MRO, when in reality it doesn't. These crop up fairly frequently; it's a bit awkward but moving `Generic[]` to the end of the bases list almost always fixes it. (We should probably offer an autofix that does that, if we can detect that doing so would fix the `inconsistent-mro` violation.)

Other type checkers do seem to issue these violations less frequently than ty (and we should look into why), but ultimately there's not much to be done about these. Typeshed _has_ to pretend that `dict` has `Generic` in its MRO, and we have to believe what typeshed tells us.

I'm actually more concerned about the autosplit diagnostic. I don't think overriding `dict.copy` on a `TypedDict` class should trigger an `invalid-method-override` violation. It should trigger _a_ violation, but it should be a violation of a separate rule that complains about any method being defined on a `TypedDict` class, because any method on a `TypedDict` class is illegal (https://github.com/astral-sh/ty/issues/2277)

---

_Comment by @charliermarsh on 2026-01-02 20:50_

Will look into it!

---

_Comment by @charliermarsh on 2026-01-02 21:02_

@oconnor663 is part-way through https://github.com/astral-sh/ty/issues/2277 so I'll just wait for that then rebase.

---

_Comment by @charliermarsh on 2026-01-02 21:10_

For the generic issue... would it make sense to add something like the following to `maybe_add_generic`, like we do for `GenericAlias`?

```rust
if remaining_bases.contains(&Type::SpecialForm(SpecialFormType::TypedDict)) {
    return;  // dict (from TypedDict) will bring in Generic
}
```

---

_Comment by @AlexWaygood on 2026-01-02 21:18_

I don't think so -- that doesn't look like it's what the runtime does?

The issue is that if `dict` actually had the MRO typeshed says it does, then it really would cause an exception at runtime to do what discord.py is doing there. So we're accurately emulating the runtime semantics:

```pycon
>>> from typing import Generic, TypeVar
>>> K = TypeVar("K")
>>> V = TypeVar("V")
>>> class dict(Generic[K, V]): ...
... 
>>> BoolOrNoneT = TypeVar('BoolOrNoneT', bound=bool|None)
>>> class TypedDict(dict): ...
... 
>>> class _BasePermissionsKwargs(Generic[BoolOrNoneT], TypedDict): ...
... 
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    class _BasePermissionsKwargs(Generic[BoolOrNoneT], TypedDict): ...
TypeError: Cannot create a consistent method resolution order (MRO) for bases Generic, TypedDict
```

---

_Comment by @oconnor663 on 2026-01-03 05:13_

> @oconnor663 is part-way through [astral-sh/ty#2277](https://github.com/astral-sh/ty/issues/2277) so I'll just wait for that then rebase.

https://github.com/astral-sh/ruff/pull/22351

---

_Marked ready for review by @charliermarsh on 2026-01-05 19:42_

---

_Review requested from @carljm by @charliermarsh on 2026-01-05 19:42_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-05 19:42_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-05 19:42_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-05 19:42_

---

_Comment by @codspeed-hq[bot] on 2026-01-05 19:50_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie%2Ftyped-mro?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22346 will **not alter performance**

<sub>Comparing <code>charlie/typed-mro</code> (44a938a) with <code>main</code> (ce059c4)</sub>



### Summary

`✅ 23` untouched  
`⏩ 30` skipped[^skipped]  




[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Ftyped-mro?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @AlexWaygood on 2026-01-09 14:45_

I do like that this emulates the runtime semantics more closely when it comes to what inheriting from `TypedDict` actually _does_ at runtime (it creates a `dict` subclass!). That means that we understand the MROs of these classes better, so we're more likely to emit accurate `inconsistent-mro` and `metaclass-conflict` diagnostics. The new `discord.py` diagnostic honestly seems like a feature in this respect, to me, even though it's strictly-speaking a false positives. (It's certainly evidence that users really should always put `Generic[T]` last in the bases list, which I think is what the typing docs recommend!)

> enables proper override checking

This I'm less sure about. Attribute and method lookup on `TypedDict` types can _never_ fallback to attributes/methods on `dict`; that would be unsound and would defeat the point of `TypedDict` types altogether. ISTM that this PR probably means that we'll need more special-casing, not less, in the long run, when it comes to override checks, member-lookup, etc.. We'll need to take special care to avoid the `dict` superclass when traversing the MRO and instead fallback to `Mapping` and/or `TypedDictFallback` where appropriate.

I'm also pretty concerned that there still seems to be a [4% regression on pydantic, and a 2% regression on colour-science, here](https://codspeed.io/astral-sh/ruff/branches/charlie%2Ftyped-mro?utm_source=github&utm_medium=comment&utm_content=header).

I haven't taken a look at your followup PR for functional typeddicts yet, and maybe that'll make it clearer to me what the benefits of this are, but at the moment I'm a _little_ hesitant about this.

---

_Comment by @AlexWaygood on 2026-01-09 14:50_

> This I'm less sure about. Attribute and method lookup on `TypedDict` types can _never_ fallback to attributes/methods on `dict`; that would be unsound and would defeat the point of `TypedDict` types altogether. ISTM that this PR probably means that we'll need more special-casing, not less, in the long run, when it comes to override checks. We'll need to take special care to avoid the `dict` superclass when traversing the MRO and instead fallback to `Mapping` and/or `TypedDictFallback` where appropriate.

The "weirdness" of `ClassBase::TypedDict` feels like it helps us avoid bugs here on `main`, because it's very obvious that you can't grab an attribute or a method off a non-class `ClassBase` -- it forces you to treat attribute/method lookup on these `ClassBase`s differently.

---

_Comment by @charliermarsh on 2026-01-09 14:56_

Okay, I can revisit later when we get to functional `TypedDict` if the value presents itself again. As an example, I believe I was able to remove all of the `ClassMemberResult::TypedDict` handling in https://github.com/astral-sh/ruff/pull/22291 so the MRO code became much simpler. I _think_ it also fixed a few bugs in the functional `TypedDict` PR, but I'm struggling to remember the details so I don't need to push on it for now.


---

_Converted to draft by @charliermarsh on 2026-01-10 18:53_

---
