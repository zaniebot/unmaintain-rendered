```yaml
number: 22128
title: "[ty] Fix `@staticmethod` combined with other decorators incorrectly binding `self`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/static-like
created_at: 2025-12-21T14:47:08Z
updated_at: 2025-12-24T03:35:10Z
url: https://github.com/astral-sh/ruff/pull/22128
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Fix `@staticmethod` combined with other decorators incorrectly binding `self`

---

_Pull request opened by @charliermarsh on 2025-12-21 14:47_

## Summary

We already had `CallableTypeKind::ClassMethodLike` to track callables that behave like `classmethods` (always bind the first argument). This PR adds the symmetric `CallableTypeKind::StaticMethodLike` for callables that behave like `staticmethods` (never bind `self`).

Closes https://github.com/astral-sh/ty/issues/2114.


---

_Label `ty` added by @charliermarsh on 2025-12-21 14:47_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 14:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-21 14:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
cki-lib (https://gitlab.com/cki-project/cki-lib)
+ tests/test_yaml.py:255:78: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `None | Literal["a: {pooh: bear}", "1", "a: {crow: bar, pooh: !reference [a, crow]}"]`
- tests/test_yaml.py:255:78: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
- tests/test_yaml.py:285:37: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
- tests/test_yaml.py:300:37: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
- Found 242 diagnostics
+ Found 240 diagnostics

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

mypy (https://github.com/python/mypy)
+ mypy/typeshed/stdlib/_frozen_importlib.pyi:76:13: error[invalid-method-override] Invalid override of method `module_repr`: Definition is incompatible with `Loader.module_repr`
+ mypy/typeshed/stdlib/_frozen_importlib.pyi:81:13: error[invalid-method-override] Invalid override of method `exec_module`: Definition is incompatible with `InspectLoader.exec_module`
+ mypy/typeshed/stdlib/_frozen_importlib.pyi:124:9: error[invalid-method-override] Invalid override of method `exec_module`: Definition is incompatible with `InspectLoader.exec_module`
+ mypy/typeshed/stdlib/_frozen_importlib_external.pyi:57:13: error[invalid-method-override] Invalid override of method `invalidate_caches`: Definition is incompatible with `MetaPathFinder.invalidate_caches`
- Found 1780 diagnostics
+ Found 1784 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/compilers/objc.py:47:9: error[invalid-method-override] Invalid override of method `get_display_language`: Definition is incompatible with `Compiler.get_display_language`
+ mesonbuild/compilers/objcpp.py:48:9: error[invalid-method-override] Invalid override of method `get_display_language`: Definition is incompatible with `Compiler.get_display_language`
- Found 1947 diagnostics
+ Found 1949 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/net/activators.py:237:9: error[invalid-method-override] Invalid override of method `bring_up_interfaces`: Definition is incompatible with `NetworkActivator.bring_up_interfaces`
+ cloudinit/net/activators.py:251:9: error[invalid-method-override] Invalid override of method `bring_up_all_interfaces`: Definition is incompatible with `NetworkActivator.bring_up_all_interfaces`
+ cloudinit/net/activators.py:298:9: error[invalid-method-override] Invalid override of method `bring_up_all_interfaces`: Definition is incompatible with `NetworkActivator.bring_up_all_interfaces`
+ cloudinit/sources/DataSourceCloudCIX.py:93:9: error[invalid-method-override] Invalid override of method `ds_detect`: Definition is incompatible with `DataSource.ds_detect`
+ cloudinit/sources/DataSourceCloudSigma.py:34:9: error[invalid-method-override] Invalid override of method `ds_detect`: Definition is incompatible with `DataSource.ds_detect`
+ cloudinit/sources/DataSourceCloudStack.py:194:9: error[invalid-method-override] Invalid override of method `ds_detect`: Definition is incompatible with `DataSource.ds_detect`
+ cloudinit/sources/DataSourceExoscale.py:136:9: error[invalid-method-override] Invalid override of method `ds_detect`: Definition is incompatible with `DataSource.ds_detect`
+ cloudinit/sources/DataSourceLXD.py:197:9: error[invalid-method-override] Invalid override of method `ds_detect`: Definition is incompatible with `DataSource.ds_detect`
+ cloudinit/sources/DataSourceNWCS.py:124:9: error[invalid-method-override] Invalid override of method `ds_detect`: Definition is incompatible with `DataSource.ds_detect`
+ cloudinit/sources/DataSourceOracle.py:175:9: error[invalid-method-override] Invalid override of method `ds_detect`: Definition is incompatible with `DataSource.ds_detect`
+ cloudinit/sources/DataSourceScaleway.py:238:9: error[invalid-method-override] Invalid override of method `ds_detect`: Definition is incompatible with `DataSource.ds_detect`
+ cloudinit/sources/DataSourceVultr.py:48:9: error[invalid-method-override] Invalid override of method `ds_detect`: Definition is incompatible with `DataSource.ds_detect`
- Found 1184 diagnostics
+ Found 1196 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ tests/contrib/aws_lambda/handlers.py:39:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `type.__call__`
+ tests/tracer/test_writer.py:718:9: error[invalid-method-override] Invalid override of method `log_message`: Definition is incompatible with `BaseHTTPRequestHandler.log_message`
- Found 8401 diagnostics
+ Found 8403 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
+ django-stubs/contrib/gis/geos/prototypes/coordseq.pyi:20:9: error[invalid-method-override] Invalid override of method `errcheck`: Definition is incompatible with `GEOSFuncFactory.errcheck`
- Found 443 diagnostics
+ Found 444 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/backends/duckdb/converter.py:19:13: error[invalid-method-override] Invalid override of method `convert_Array`: Definition is incompatible with `PandasData.convert_Array`
- Found 4608 diagnostics
+ Found 4609 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2803 diagnostics
+ Found 2806 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/models/hland/hland_masks.py:15:9: error[invalid-method-override] Invalid override of method `get_refindices`: Definition is incompatible with `IndexMask.get_refindices`
+ hydpy/models/lland/lland_masks.py:48:9: error[invalid-method-override] Invalid override of method `get_refindices`: Definition is incompatible with `IndexMask.get_refindices`
+ hydpy/models/whmod/whmod_masks.py:21:9: error[invalid-method-override] Invalid override of method `get_refindices`: Definition is incompatible with `IndexMask.get_refindices`
+ hydpy/models/whmod/whmod_masks.py:108:9: error[invalid-method-override] Invalid override of method `get_refindices`: Definition is incompatible with `IndexMask.get_refindices`
+ hydpy/models/wland/wland_masks.py:22:9: error[invalid-method-override] Invalid override of method `get_refindices`: Definition is incompatible with `IndexMask.get_refindices`
- Found 680 diagnostics
+ Found 685 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/core/numbers.py:2845:9: error[invalid-method-override] Invalid override of method `__abs__`: Definition is incompatible with `Integer.__abs__`
+ sympy/core/numbers.py:2849:9: error[invalid-method-override] Invalid override of method `__neg__`: Definition is incompatible with `Integer.__neg__`
+ sympy/core/numbers.py:2908:9: error[invalid-method-override] Invalid override of method `__abs__`: Definition is incompatible with `Integer.__abs__`
+ sympy/core/numbers.py:2912:9: error[invalid-method-override] Invalid override of method `__neg__`: Definition is incompatible with `Integer.__neg__`
+ sympy/core/numbers.py:2922:9: error[invalid-method-override] Invalid override of method `factors`: Definition is incompatible with `Rational.factors`
+ sympy/core/numbers.py:2964:9: error[invalid-method-override] Invalid override of method `__abs__`: Definition is incompatible with `Integer.__abs__`
+ sympy/core/numbers.py:2968:9: error[invalid-method-override] Invalid override of method `__neg__`: Definition is incompatible with `Integer.__neg__`
+ sympy/core/numbers.py:3022:9: error[invalid-method-override] Invalid override of method `__abs__`: Definition is incompatible with `Rational.__abs__`
+ sympy/core/numbers.py:3555:9: error[invalid-method-override] Invalid override of method `__abs__`: Definition is incompatible with `Expr.__abs__`
+ sympy/core/numbers.py:3565:9: error[invalid-method-override] Invalid override of method `__neg__`: Definition is incompatible with `Expr.__neg__`
+ sympy/core/numbers.py:3684:9: error[invalid-method-override] Invalid override of method `__abs__`: Definition is incompatible with `Expr.__abs__`
+ sympy/core/numbers.py:3847:9: error[invalid-method-override] Invalid override of method `__abs__`: Definition is incompatible with `Expr.__abs__`
+ sympy/core/numbers.py:4161:9: error[invalid-method-override] Invalid override of method `__abs__`: Definition is incompatible with `Expr.__abs__`
- Found 15345 diagnostics
+ Found 15358 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/db/solanatx.py:99:9: error[invalid-method-override] Invalid override of method `get_transactions`: Definition is incompatible with `DBCommonTx.get_transactions`
- Found 2095 diagnostics
+ Found 2096 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5104 diagnostics
+ Found 5107 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14412 diagnostics
+ Found 14413 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-21 15:48_

A lot of new diagnostics but AFAICT they are correct based on how we intend to enforce Liskov per `liskov.md`? For example, in SymPy, `class Integer` defines `__abs__` as an instance method:

https://github.com/sympy/sympy/blob/221edeeada0fa2b20e89d093e1f833efec4dce30/sympy/core/numbers.py#L1890

But the subclass `Zero` defines it as a static method:

https://github.com/sympy/sympy/blob/221edeeada0fa2b20e89d093e1f833efec4dce30/sympy/core/numbers.py#L2844C1-L2846C22

---

_Marked ready for review by @charliermarsh on 2025-12-21 15:48_

---

_Review requested from @carljm by @charliermarsh on 2025-12-21 15:48_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-21 15:48_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-21 15:48_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-21 15:48_

---

_Comment by @AlexWaygood on 2025-12-21 15:51_

You need to update an assertion in the benchmarks:

>   thread 'main' (4942) panicked at crates/ruff_benchmark/benches/ty_walltime.rs:78:5:
>   Expected between 1 and 13100 diagnostics on project 'sympy' but got 13106

---

_Comment by @charliermarsh on 2025-12-21 15:53_

Thank you!

---

_Comment by @AlexWaygood on 2025-12-21 15:55_

It would be great if that was a snapshot or something, but I think then it would be hard for it to serve its intended purpose of checking that the benchmark itself was working correctly :/

---

_Comment by @charliermarsh on 2025-12-21 15:56_

(I think I noticed it last time when I clicked in, but I must not have looked closely enough this time, I thought it was spurious.)

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 16:10_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@MichaReiser reviewed on 2025-12-22 08:44_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty_walltime.rs`:197 on 2025-12-22 08:44_

Feel free to bump them higher next time. E.g. 13200 or similar. The assertions only exist that we don't break the benchmarks accidentially

---

_Comment by @carljm on 2025-12-24 01:23_

> `class Integer` defines `__abs__` as an instance method

Pyright errors on this, Pyrefly and mypy don't. It's a bit of a gray area -- when called on an instance, that's a perfectly compatible override, but if called on a type, it's not.

The rationale for not erroring on this is that calling normal methods on types is totally Liskov-broken anyway, because `self` is always overridden in a Liskov-incompatible way by default. So arguably we should error on calling normal methods on types (passing in `self` manually), but not error on this override.

But either way, this is something we can handle separately in the Liskov work.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/final.md`:127 on 2025-12-24 01:28_

I'd actually be inclined to keep a TODO here that maybe this _shouldn't_ be a Liskov violation, but I'd like to hear what @AlexWaygood thinks.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/override.md`:191 on 2025-12-24 01:30_

I'm pretty sure this one should not be an invalid override -- I don't think there is any way you can call this method where this override would cause a problem? Whether called on the type or on an instance, it will behave the same from the caller POV -- the only difference is whether the method body gets access to `cls`, which is not a concern of Liskov.

---

_@carljm approved on 2025-12-24 01:32_

Nice!

---

_@carljm reviewed on 2025-12-24 01:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/override.md`:191 on 2025-12-24 01:32_

(But I don't know that we need to do anything more about it in this PR than leave a TODO comment)

---

_@charliermarsh reviewed on 2025-12-24 03:23_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/override.md`:191 on 2025-12-24 03:23_

Done.

---

_@charliermarsh reviewed on 2025-12-24 03:23_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/final.md`:127 on 2025-12-24 03:23_

Done.

---

_Merged by @charliermarsh on 2025-12-24 03:35_

---

_Closed by @charliermarsh on 2025-12-24 03:35_

---

_Branch deleted on 2025-12-24 03:35_

---
