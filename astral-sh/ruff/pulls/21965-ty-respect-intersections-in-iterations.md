```yaml
number: 21965
title: "[ty] Respect intersections in iterations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/int
created_at: 2025-12-13T19:28:41Z
updated_at: 2025-12-19T17:36:42Z
url: https://github.com/astral-sh/ruff/pull/21965
synced_at: 2026-01-12T15:57:37Z
```

# [ty] Respect intersections in iterations

---

_@charliermarsh_

## Summary

This PR implements the strategy described in https://github.com/astral-sh/ty/issues/1871: we iterate over the positive types, resolve them, then intersect the results.


---

_Comment by @astral-sh-bot[bot] on 2025-12-13 19:30_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-13 19:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kornia (https://github.com/kornia/kornia)
- kornia/augmentation/container/augment.py:405:35: error[unresolved-attribute] Object of type `object` has no attribute `type`
- kornia/augmentation/container/augment.py:414:35: error[unresolved-attribute] Object of type `object` has no attribute `type`
- Found 765 diagnostics
+ Found 763 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/spec.py:2727:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[Spec, int]`, found `tuple[Spec | str, int]`
- lib/spack/spack/spec.py:2727:76: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Spec | str`
- Found 4297 diagnostics
+ Found 4295 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/rich/text.py:397:24: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Text | str`, found `str | Style`
- Found 613 diagnostics
+ Found 612 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/test.py:436:38: error[invalid-argument-type] Argument to bound method `add_file` is incorrect: Expected `str | PathLike[str] | IO[bytes] | FileStorage`, found `bytes | IO[bytes] | str`
- src/werkzeug/test.py:436:38: error[invalid-argument-type] Argument to bound method `add_file` is incorrect: Expected `str | None`, found `IO[bytes] | str`
+ src/werkzeug/test.py:436:38: error[invalid-argument-type] Argument to bound method `add_file` is incorrect: Expected `str | None`, found `bytes | IO[bytes] | str`
- src/werkzeug/test.py:436:38: error[invalid-argument-type] Argument to bound method `add_file` is incorrect: Expected `str | None`, found `IO[bytes] | str`
+ src/werkzeug/test.py:436:38: error[invalid-argument-type] Argument to bound method `add_file` is incorrect: Expected `str | None`, found `bytes | IO[bytes] | str`
- Found 386 diagnostics
+ Found 387 diagnostics

rich (https://github.com/Textualize/rich)
- rich/text.py:397:24: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Text | str`, found `str | Style`
- Found 349 diagnostics
+ Found 348 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/porcelain/__init__.py:3539:41: error[invalid-argument-type] Argument to function `_make_branch_ref` is incorrect: Expected `str | bytes`, found `Unknown | (Sequence[str | bytes] & ~Top[list[Unknown]] & ~tuple[object, ...]) | bytes`
+ dulwich/porcelain/__init__.py:3539:41: error[invalid-argument-type] Argument to function `_make_branch_ref` is incorrect: Expected `str | bytes`, found `(Sequence[str | bytes] & ~Top[list[Unknown]] & ~tuple[object, ...]) | bytes | Unknown`

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/globals.py:81:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[@Todo, ...] | (Any & ~Top[list[Unknown]])` and value of type `Any` on object of type `dict[T@TokenManager, Any]`
+ boostedblob/globals.py:81:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Any, ...] | (Any & ~Top[list[Unknown]])` and value of type `Any` on object of type `dict[T@TokenManager, Any]`
- boostedblob/globals.py:82:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[@Todo, ...] | (Any & ~Top[list[Unknown]])` and value of type `Any` on object of type `dict[T@TokenManager, int | float]`
+ boostedblob/globals.py:82:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Any, ...] | (Any & ~Top[list[Unknown]])` and value of type `Any` on object of type `dict[T@TokenManager, int | float]`

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/filepost.py:48:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `str`, found `str | bytes | tuple[str, str | bytes] | tuple[str, str | bytes, str]`
- src/urllib3/filepost.py:48:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `((str, str | bytes, /) -> str) | None`, found `str | bytes | tuple[str, str | bytes] | tuple[str, str | bytes, str]`
- Found 268 diagnostics
+ Found 266 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/structure/nav.py:223:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `@Todo | None`
+ mkdocs/structure/nav.py:223:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ pymongo/helpers_shared.py:171:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, int]`, found `tuple[str, int | str | Mapping[str, Any]]`
+ pymongo/helpers_shared.py:191:21: error[invalid-argument-type] Method `__getitem__` of type `bound method Mapping[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `str | tuple[str, int | str | Mapping[str, Any]]` on object of type `Mapping[str, Any]`
+ pymongo/helpers_shared.py:193:13: error[invalid-assignment] Invalid subscript assignment with key of type `str | tuple[str, int | str | Mapping[str, Any]]` and value of type `@Todo` on object of type `dict[str, Any]`
- Found 446 diagnostics
+ Found 449 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/cwlprov/provenance_profile.py:563:37: error[invalid-argument-type] Argument to bound method `used_artefacts` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements] | list[MutableMapping[str, None | int | str | ... omitted 3 union elements]]`, found `str | MutableMapping[str, None | int | str | ... omitted 3 union elements]`
+ cwltool/cwlprov/provenance_profile.py:592:43: error[invalid-argument-type] Argument to bound method `generate_output_prov` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements] | MutableSequence[MutableMapping[str, None | int | str | ... omitted 3 union elements]] | None`, found `str | MutableMapping[str, None | int | str | ... omitted 3 union elements]`
+ cwltool/process.py:321:47: error[invalid-argument-type] Argument to function `_collectDirEntries` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements] | MutableSequence[MutableMapping[str, None | int | str | ... omitted 3 union elements]] | None`, found `str | MutableMapping[str, None | int | str | ... omitted 3 union elements]`
- cwltool/utils.py:186:13: error[invalid-assignment] Invalid subscript assignment with key of type `str | @Todo` and value of type `str | MutableSequence[Any] | MutableMapping[str, Any]` on object of type `MutableSequence[Any]`
+ cwltool/utils.py:186:13: error[invalid-assignment] Invalid subscript assignment with key of type `str | Any` and value of type `str | MutableSequence[Any] | MutableMapping[str, Any]` on object of type `MutableSequence[Any]`
- Found 242 diagnostics
+ Found 245 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/scikit_build_core/metadata/__init__.py:94:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 44 diagnostics
+ Found 42 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- arviz/data/io_pystan.py:151:28: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[@Todo | str, @Todo | str]`
+ arviz/data/io_pystan.py:151:28: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[Unknown | str, Unknown | str]`
- arviz/data/io_pystan.py:157:48: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[@Todo | str, @Todo | str]`
+ arviz/data/io_pystan.py:157:48: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[Unknown | str, Unknown | str]`
- arviz/data/io_pystan.py:163:48: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[@Todo | str, @Todo | str]`
+ arviz/data/io_pystan.py:163:48: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[Unknown | str, Unknown | str]`
- arviz/data/io_pystan.py:437:28: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[@Todo | str, @Todo | str]`
+ arviz/data/io_pystan.py:437:28: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[Unknown | str, Unknown | str]`
- arviz/data/io_pystan.py:443:48: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[@Todo | str, @Todo | str]`
+ arviz/data/io_pystan.py:443:48: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[Unknown | str, Unknown | str]`
- arviz/data/io_pystan.py:448:48: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[@Todo | str, @Todo | str]`
+ arviz/data/io_pystan.py:448:48: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Unknown & ~str & ~Top[list[Unknown]] & ~tuple[object, ...]) | None | dict[Unknown | str, Unknown | str]`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/plotting/_plot.py:106:32: error[invalid-argument-type] Argument is incorrect: Expected `int | float | datetime | timedelta`, found `int | float | str | (@Todo & ~None) | IntrinsicType`
+ src/bokeh/plotting/_plot.py:106:32: error[invalid-argument-type] Argument is incorrect: Expected `int | float | datetime | timedelta`, found `int | float | str | (Unknown & ~None) | IntrinsicType`
- src/bokeh/plotting/_plot.py:106:45: error[invalid-argument-type] Argument is incorrect: Expected `int | float | datetime | timedelta`, found `int | float | str | (@Todo & ~None) | IntrinsicType`
+ src/bokeh/plotting/_plot.py:106:45: error[invalid-argument-type] Argument is incorrect: Expected `int | float | datetime | timedelta`, found `int | float | str | (Unknown & ~None) | IntrinsicType`

pandas (https://github.com/pandas-dev/pandas)
- pandas/tests/extension/date/array.py:99:31: warning[redundant-cast] Value is already of type `ndarray[tuple[Any, ...], dtype[Any]]`
- Found 3736 diagnostics
+ Found 3735 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/lib/rest.py:153:27: error[unresolved-attribute] Object of type `((...) -> HttpResponse) | set[str]` has no attribute `__name__`
+ zerver/lib/rest.py:153:27: error[unresolved-attribute] Object of type `(...) -> HttpResponse` has no attribute `__name__`
- zerver/lib/rest.py:164:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["narrow_user_session_cache"]` and `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:170:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["override_api_url_scheme"]` and `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:176:80: error[invalid-argument-type] Argument is incorrect: Expected `(...) -> HttpResponse`, found `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:177:10: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["override_api_url_scheme"]` and `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:182:83: error[invalid-argument-type] Argument is incorrect: Expected `(...) -> HttpResponse`, found `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:187:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["override_api_url_scheme"]` and `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:189:64: error[invalid-argument-type] Argument to function `authenticated_json_view` is incorrect: Expected `(...) -> HttpResponse`, found `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:197:34: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["allow_incoming_webhooks"]` and `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:198:11: error[invalid-argument-type] Argument is incorrect: Expected `(...) -> HttpResponse`, found `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:201:13: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["allow_anonymous_user_web"]` and `((...) -> HttpResponse) | set[str]`
- zerver/lib/rest.py:205:57: error[invalid-argument-type] Argument to function `public_json_view` is incorrect: Expected `(...) -> HttpResponse`, found `((...) -> HttpResponse) | set[str]`
- zilencer/auth.py:170:45: error[invalid-argument-type] Argument to function `authenticated_remote_server_view` is incorrect: Expected `(...) -> HttpResponse`, found `((...) -> HttpResponse) | set[str]`
- Found 3676 diagnostics
+ Found 3664 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/numpy/tensor_contractions.py:548:46: error[invalid-argument-type] Argument to function `canonicalize_axis` is incorrect: Expected `SupportsIndex`, found `int | Sequence[int]`
+ jax/_src/numpy/tensor_contractions.py:549:46: error[invalid-argument-type] Argument to function `canonicalize_axis` is incorrect: Expected `SupportsIndex`, found `int | Sequence[int]`
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2799 diagnostics
+ Found 2804 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- Found 1838 diagnostics
+ Found 1839 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/geometry/line.py:2544:26: warning[possibly-missing-attribute] Attribute `x` may be missing on object of type `@Todo | Point`
+ sympy/geometry/line.py:2544:26: warning[possibly-missing-attribute] Attribute `x` may be missing on object of type `(Unknown & Basic) | Point`
- sympy/geometry/line.py:2544:53: warning[possibly-missing-attribute] Attribute `y` may be missing on object of type `@Todo | Point`
+ sympy/geometry/line.py:2544:53: warning[possibly-missing-attribute] Attribute `y` may be missing on object of type `(Unknown & Basic) | Point`
- sympy/geometry/line.py:2545:26: warning[possibly-missing-attribute] Attribute `z` may be missing on object of type `@Todo | Point`
+ sympy/geometry/line.py:2545:26: warning[possibly-missing-attribute] Attribute `z` may be missing on object of type `(Unknown & Basic) | Point`
- sympy/geometry/line.py:2726:26: warning[possibly-missing-attribute] Attribute `x` may be missing on object of type `@Todo | Point`
+ sympy/geometry/line.py:2726:26: warning[possibly-missing-attribute] Attribute `x` may be missing on object of type `(Unknown & Basic) | Point`
- sympy/geometry/line.py:2726:53: warning[possibly-missing-attribute] Attribute `y` may be missing on object of type `@Todo | Point`
+ sympy/geometry/line.py:2726:53: warning[possibly-missing-attribute] Attribute `y` may be missing on object of type `(Unknown & Basic) | Point`
- sympy/geometry/line.py:2727:26: warning[possibly-missing-attribute] Attribute `z` may be missing on object of type `@Todo | Point`
+ sympy/geometry/line.py:2727:26: warning[possibly-missing-attribute] Attribute `z` may be missing on object of type `(Unknown & Basic) | Point`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2093 diagnostics
+ Found 2091 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/template/light.py:738:13: error[invalid-assignment] Object of type `tuple[None | int | @Todo, None | int | @Todo, None | int | @Todo]` is not assignable to attribute `_attr_rgb_color` of type `tuple[int, int, int] | None`
+ homeassistant/components/template/light.py:738:13: error[invalid-assignment] Object of type `tuple[None | int | Unknown, None | int | Unknown, None | int | Unknown]` is not assignable to attribute `_attr_rgb_color` of type `tuple[int, int, int] | None`
- homeassistant/components/template/light.py:783:13: error[invalid-assignment] Object of type `tuple[None | int | @Todo, None | int | @Todo, None | int | @Todo, None | int | @Todo]` is not assignable to attribute `_attr_rgbw_color` of type `tuple[int, int, int, int] | None`
+ homeassistant/components/template/light.py:783:13: error[invalid-assignment] Object of type `tuple[None | int | Unknown, None | int | Unknown, None | int | Unknown, None | int | Unknown]` is not assignable to attribute `_attr_rgbw_color` of type `tuple[int, int, int, int] | None`
- homeassistant/components/template/light.py:829:13: error[invalid-assignment] Object of type `tuple[None | int | @Todo, None | int | @Todo, None | int | @Todo, None | int | @Todo, None | int | @Todo]` is not assignable to attribute `_attr_rgbww_color` of type `tuple[int, int, int, int, int] | None`
+ homeassistant/components/template/light.py:829:13: error[invalid-assignment] Object of type `tuple[None | int | Unknown, None | int | Unknown, None | int | Unknown, None | int | Unknown, None | int | Unknown]` is not assignable to attribute `_attr_rgbww_color` of type `tuple[int, int, int, int, int] | None`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14420 diagnostics
+ Found 14419 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
+ misc/python/materialize/feature_benchmark/benchmark.py:119:17: warning[possibly-missing-attribute] Attribute `run` may be missing on object of type `None | Action | Unknown`
+ misc/python/materialize/feature_benchmark/benchmark.py:131:17: warning[possibly-missing-attribute] Attribute `run` may be missing on object of type `None | Action | Unknown`
+ misc/python/materialize/feature_benchmark/benchmark.py:142:17: warning[possibly-missing-attribute] Attribute `run` may be missing on object of type `None | Action | Unknown`
- Found 309 diagnostics
+ Found 312 diagnostics

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

_Renamed from "Respect intersections in iterations" to "[ty] Respect intersections in iterations" by @charliermarsh on 2025-12-13 19:42_

---

_Label `ty` added by @charliermarsh on 2025-12-13 19:42_

---

_Comment by @charliermarsh on 2025-12-13 20:07_

The ecosystem changes in Materialize seems incorrect, hmm...

---

_Comment by @carljm on 2025-12-13 20:43_

Those diagnostics on materialize are correct, though arguably pedantic. They are us recognizing that it is possible for there to be a common subtype of `Action` and `list`, so the `isinstance(shared, list)` does not actually rule out `shared` being a subclass of `Action`. And `Action` has an `__iter__` method that returns `Iterator[None]`.

The fix for this code would be to check `isinstance(shared, Action)` instead of `isinstance(shared, list)`. (Or make `Action` `@final`.)

We could debate whether ty's pedantic correctness is _useful_ in this kind of scenario (https://github.com/astral-sh/ty/issues/1578 tracks this), but in any case, it's not a problem for this PR.

---

_@carljm reviewed on 2025-12-13 20:49_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6622 on 2025-12-13 20:49_

I don't think short-circuiting here (or above, for the first element) is correct. The error-handling logic here needs to be "collect all successful results, ignoring failed ones, and intersect the successful ones". We should only fail iteration if _all_ intersection elements fail to iterate.

We should add some tests where some intersection elements fail to iterate and others succeed. And some where all intersection elements fail.

---

_Marked ready for review by @charliermarsh on 2025-12-13 22:10_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-13 22:10_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-13 22:10_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-13 22:10_

---

_@AlexWaygood reviewed on 2025-12-13 22:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1806 on 2025-12-13 22:21_

this branch doesn't have any test coverage yet -- I can apply this diff to your branch, and no tests fail:

```diff
diff --git a/crates/ty_python_semantic/src/types/tuple.rs b/crates/ty_python_semantic/src/types/tuple.rs
index e551871539..566385ad14 100644
--- a/crates/ty_python_semantic/src/types/tuple.rs
+++ b/crates/ty_python_semantic/src/types/tuple.rs
@@ -1799,27 +1799,12 @@ impl<'db> TupleSpecBuilder<'db> {
     /// `tuple[int, str, bytes]`, the result will be a tuple-spec builder for
     /// `tuple[int & str & bytes, ...]`.
     pub(crate) fn intersect(mut self, db: &'db dyn Db, other: &TupleSpec<'db>) -> Self {
-        match (&mut self, other) {
-            (TupleSpecBuilder::Fixed(our_elements), TupleSpec::Fixed(new_elements))
-                if our_elements.len() == new_elements.len() =>
-            {
-                for (existing, new) in our_elements.iter_mut().zip(new_elements.elements()) {
-                    *existing = IntersectionType::from_elements(db, [*existing, *new]);
-                }
-                self
-            }
-
-            _ => {
-                let intersected = IntersectionType::from_elements(
-                    db,
-                    self.all_elements().chain(other.all_elements()),
-                );
-                TupleSpecBuilder::Variable {
-                    prefix: vec![],
-                    variable: intersected,
-                    suffix: vec![],
-                }
-            }
+        let intersected =
+            IntersectionType::from_elements(db, self.all_elements().chain(other.all_elements()));
+        TupleSpecBuilder::Variable {
+            prefix: vec![],
+            variable: intersected,
+            suffix: vec![],
         }
     }
```

and in fact, I can't immediately think of a test that fails with that diff applied so -- unless you can -- that might just be a good simplification to make?

---

_@AlexWaygood reviewed on 2025-12-13 22:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1806 on 2025-12-13 22:31_

I tried writing this test to exercise the logic in this branch, but it doesn't look like it has the correct results right now...

```py
from collections.abc import Iterator

class Foo:
    def __iter__(self) -> Iterator[object]:
        raise NotImplementedError

def f(x: tuple[int, str, bytes]):
    if isinstance(x, Foo):
        a, b, c = x
        reveal_type(a)  # revealed: Unknown
        reveal_type(tuple(x))  # revealed: tuple[()]
```

that might be due to pre-existing issues rather than a bug in your PR, though -- not sure. We get those bad revealed types on `main` too.

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-15 18:50_

---

_Comment by @astral-sh-bot[bot] on 2025-12-15 18:59_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 7 | 13 | 6 |
| `possibly-missing-attribute` | 3 | 1 | 12 |
| `invalid-assignment` | 1 | 0 | 6 |
| `invalid-return-type` | 0 | 2 | 4 |
| `unsupported-operator` | 0 | 6 | 0 |
| `unresolved-attribute` | 0 | 2 | 1 |
| `redundant-cast` | 0 | 1 | 0 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **11** | **26** | **29** |

**[Full report with detailed diff](https://charlie-int.ecosystem-663.pages.dev/diff)** ([timing results](https://charlie-int.ecosystem-663.pages.dev/timing))




---

_@charliermarsh reviewed on 2025-12-19 01:39_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/tuple.rs`:1806 on 2025-12-19 01:39_

Okay, added handling for some other cases:

- Fixed-length tuple with a homogenous iterable (your example here)
- Two variable-length, homogenous iterables

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-19 01:39_

---

_Review requested from @carljm by @charliermarsh on 2025-12-19 01:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1796 on 2025-12-19 15:03_

My concern here is that if you have two tuples `X` and `Y`, the tuple spec of `X & Y` should _always_ be narrower than either the tuple spec for `X` or the tuple spec for `Y`. The last branch of this method violates that, I think: you could end up with a spec from the intersected tuples that's broader than one of the two tuples being thrown into the intersection, which is going to lead to some seriously weird behaviour at some point 😄

Here's a version of this method that's still not totally free of TODOs, but I think is much more generalised than what you have currently:

```rs
    /// Return a new tuple-spec builder that reflects the intersection of this tuple and another tuple.
    ///
    /// For example, if `self` is a tuple-spec builder for `tuple[int, str]` and `other` is a
    /// tuple-spec for `tuple[object, object]`, the result will be a tuple-spec builder for
    /// `tuple[int, str]` (since `int & object` simplifies to `int`, and `str & object` to `str`).
    pub(crate) fn intersect(mut self, db: &'db dyn Db, other: &TupleSpec<'db>) -> Self {
        match (&mut self, other) {
            // Both fixed-length with the same length: element-wise intersection.
            (TupleSpecBuilder::Fixed(our_elements), TupleSpec::Fixed(new_elements))
                if our_elements.len() == new_elements.len() =>
            {
                for (existing, new) in our_elements.iter_mut().zip(new_elements.elements()) {
                    *existing = IntersectionType::from_elements(db, [*existing, *new]);
                }
                return self;
            }

            (TupleSpecBuilder::Fixed(our_elements), TupleSpec::Variable(var)) => {
                if let Ok(tuple) = var.resize(db, TupleLength::Fixed(our_elements.len())) {
                    return self.intersect(db, &tuple);
                }
            }

            (TupleSpecBuilder::Variable { .. }, TupleSpec::Fixed(fixed)) => {
                if let Ok(tuple) = self
                    .clone()
                    .build()
                    .resize(db, TupleLength::Fixed(fixed.len()))
                {
                    return TupleSpecBuilder::from(&tuple).intersect(db, other);
                }
            }

            (
                TupleSpecBuilder::Variable {
                    prefix,
                    variable,
                    suffix,
                },
                TupleSpec::Variable(var),
            ) => {
                if prefix.len() == var.prefix.len() && suffix.len() == var.suffix.len() {
                    for (existing, new) in prefix.iter_mut().zip(var.prefix_elements()) {
                        *existing = IntersectionType::from_elements(db, [*existing, *new]);
                    }
                    *variable = IntersectionType::from_elements(db, [*variable, var.variable]);
                    for (existing, new) in suffix.iter_mut().zip(var.suffix_elements()) {
                        *existing = IntersectionType::from_elements(db, [*existing, *new]);
                    }
                    return self;
                }

                let self_built = self.clone().build();
                let self_len = self_built.len();
                if let Ok(resized) = var.resize(db, self_len) {
                    return self.intersect(db, &resized);
                } else if let Ok(resized) = self_built.resize(db, var.len()) {
                    return TupleSpecBuilder::from(&resized).intersect(db, other);
                }
            }

            _ => {}
        }

        // TODO: probably incorrect? `tuple[int, str] & tuple[int, str, bytes]` should resolve to `Never`.
        // So maybe this function should be fallible (return an `Option`)?
        let intersected =
            IntersectionType::from_elements(db, self.all_elements().chain(other.all_elements()));
        TupleSpecBuilder::Variable {
            prefix: vec![],
            variable: intersected,
            suffix: vec![],
        }
    }
```

---

_@AlexWaygood approved on 2025-12-19 15:03_

Thanks, looks good

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-19 17:21_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-19 17:21_

---

_@AlexWaygood approved on 2025-12-19 17:32_

ecosystem report LG!

---

_Merged by @charliermarsh on 2025-12-19 17:36_

---

_Closed by @charliermarsh on 2025-12-19 17:36_

---

_Branch deleted on 2025-12-19 17:36_

---

_Comment by @charliermarsh on 2025-12-19 17:36_

Tyvm!

---
