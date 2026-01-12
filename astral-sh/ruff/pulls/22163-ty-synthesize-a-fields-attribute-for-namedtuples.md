```yaml
number: 22163
title: "[ty] Synthesize a `_fields` attribute for NamedTuples"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/fields
created_at: 2025-12-23T16:12:04Z
updated_at: 2025-12-23T21:44:45Z
url: https://github.com/astral-sh/ruff/pull/22163
synced_at: 2026-01-12T15:57:43Z
```

# [ty] Synthesize a `_fields` attribute for NamedTuples

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2176.


---

_Label `ty` added by @charliermarsh on 2025-12-23 16:12_

---

_Marked ready for review by @charliermarsh on 2025-12-23 16:12_

---

_Review requested from @carljm by @charliermarsh on 2025-12-23 16:12_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-23 16:12_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-23 16:12_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-23 16:12_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-12-23 16:12_

---

_Comment by @astral-sh-bot[bot] on 2025-12-23 16:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Renamed from "[ty] Synthesize a `_fields` method for NamedTuples" to "[ty] Synthesize a `_fields` attribute for NamedTuples" by @AlexWaygood on 2025-12-23 16:14_

---

_Comment by @astral-sh-bot[bot] on 2025-12-23 16:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 43 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5106 diagnostics
+ Found 5107 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:269 on 2025-12-23 16:15_

wouldn't `tuple[Literal["name"], Literal["age"]]` be even more precise here? Would there be any downsides to that?

---

_@AlexWaygood reviewed on 2025-12-23 16:15_

---

_@charliermarsh reviewed on 2025-12-23 16:15_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:269 on 2025-12-23 16:15_

Yeah we can do that. I wasn't sure honestly (whether there were any downsides).

---

_@AlexWaygood reviewed on 2025-12-23 16:26_

---

_Review comment by @AlexWaygood on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:193 on 2025-12-23 16:26_

I know I suggested this but... on further reflection, I think this might cause problems down the line. Most of the time you don't access the `_fields` attribute on _instances_ of a `NamedTuple` class -- most of the time you access it on the class itself. And it's a bit dubious whether `@property` members (or any non-`ClassVar` non-method members, really) should be available on the class itself. The fact that we currently allow it is a missing check in our `Protocol` machinery somewhere.

So this might be better? I think it should still fix the problem where `NamedTuple` instances weren't considered assignable to `NamedTupleLike`:

```suggestion
    # _fields is defined as `tuple[Any, ...]` rather than `tuple[str, ...]` so
    # that instances of actual `NamedTuple` classes with more precise `_fields`
    # types are considered assignable to this protocol (protocol attribute members
    # are invariant, and `tuple[str, str]` is not invariantly assignable to
    # `tuple[str, ...]`
    _fields: ClassVar[tuple[Any, ...]]
    _field_defaults: ClassVar[dict[str, Any]]
```

---

_@AlexWaygood reviewed on 2025-12-23 16:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:269 on 2025-12-23 16:35_

I can't think of any downsides, since tuples are covariant

---

_@AlexWaygood approved on 2025-12-23 16:58_

LGTM

---

_Merged by @charliermarsh on 2025-12-23 21:38_

---

_Closed by @charliermarsh on 2025-12-23 21:38_

---

_Branch deleted on 2025-12-23 21:38_

---

_Comment by @AlexWaygood on 2025-12-23 21:43_

> ## Summary
> 
> Closes #2176.

I think you meant to link to a ty issue there 😆

---
