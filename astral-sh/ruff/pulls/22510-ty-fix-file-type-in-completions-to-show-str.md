```yaml
number: 22510
title: "[ty] Fix `__file__` type in completions to show `str` instead of `str | None`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: charlie/spec
created_at: 2026-01-11T22:28:08Z
updated_at: 2026-01-12T14:20:33Z
url: https://github.com/astral-sh/ruff/pull/22510
synced_at: 2026-01-12T15:03:37Z
```

# [ty] Fix `__file__` type in completions to show `str` instead of `str | None`

---

_Pull request opened by @charliermarsh on 2026-01-11 22:28_

## Summary

The type inference system already correctly special-cases `__file__` to return `str` for the current module (since the code is executing from an existing file). However, the completion system was bypassing this logic and pulling `__file__: str | None` directly from `types.ModuleType` in typeshed.

This PR adds implicit module globals (like `__file__`, `__name__`, etc.) with their correctly-typed values to completions, reusing the existing `module_type_implicit_global_symbol` function that already handles the special-casing.

Closes https://github.com/astral-sh/ty/issues/2445.


---

_Label `server` added by @charliermarsh on 2026-01-11 22:28_

---

_Label `ty` added by @charliermarsh on 2026-01-11 22:28_

---

_Comment by @astral-sh-bot[bot] on 2026-01-11 22:29_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-11 22:31_


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

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1826 diagnostics
+ Found 1827 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @carljm by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @BurntSushi by @dhruvmanila on 2026-01-12 06:20_

---

_Comment by @sinon on 2026-01-12 09:39_

A related PR which is the semantics side for handling
`from a import __file__ as a_path`: https://github.com/astral-sh/ruff/pull/22333 - with that PR, existing current module semantic overrides and now this there might soon be at least 3 places where these special casings for `__file__` (and other per-file dunder attributes) are being specified 🤔 Maybe fine for now but probably worth raising a follow-up issue to re-examine if the 3rd case lands (as there might end up being more if users then also want the completion typing correct for the `from a import __f<CURSOR>` case too to match this)

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:250 on 2026-01-12 13:44_

Do we still need to get completions from the `builtins` module below if we're doing this?

---

_@BurntSushi reviewed on 2026-01-12 13:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:250 on 2026-01-12 13:50_

yeah, this special case is only for the 8 or so "implicit module globals" that are present in _every_ global namespace. Previously we only "accidentally" recognized them as being available in our autocompletion engine because they are present in the builtins-module global namespace just like every other module-global namespace. But treating them as builtin symbols was incorrect, because they might have more precise types in the global namespace of the _current_ module than they do in the `builtins` module's global namespace.

So for all builtins symbols other than these 8 or so special-cased symbols, we still need to import the `builtins` module and lookup the global namespace as we do below.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1780 on 2026-01-12 14:01_

```suggestion
        let special_cased = ["__builtins__", "__debug__", "__warningregistry__"]
            .into_iter()
            .map(Name::new_static);
```

---

_@AlexWaygood approved on 2026-01-12 14:01_

LGTM

---

_Merged by @charliermarsh on 2026-01-12 14:20_

---

_Closed by @charliermarsh on 2026-01-12 14:20_

---

_Branch deleted on 2026-01-12 14:20_

---
