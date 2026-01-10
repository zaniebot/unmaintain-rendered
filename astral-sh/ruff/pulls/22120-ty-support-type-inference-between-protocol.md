```yaml
number: 22120
title: "[ty] Support type inference between protocol instances"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/protocol-inference
created_at: 2025-12-20T21:48:40Z
updated_at: 2025-12-23T08:24:02Z
url: https://github.com/astral-sh/ruff/pull/22120
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Support type inference between protocol instances

---

_Pull request opened by @ibraheemdev on 2025-12-20 21:48_

## Summary

Resolves https://github.com/astral-sh/ty/issues/2060.

---

_Review requested from @carljm by @ibraheemdev on 2025-12-20 21:48_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-20 21:48_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-20 21:48_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-20 21:48_

---

_Label `ty` added by @ibraheemdev on 2025-12-20 21:48_

---

_Renamed from "[tySupport type inference between protocol instances" to "[ty] Support type inference between protocol instances" by @ibraheemdev on 2025-12-20 21:48_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 21:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-20 21:51_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- mypy_primer/model.py:189:40: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Generator[Path, None, None]`
- Found 5 diagnostics
+ Found 4 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/spec_parser.py:501:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Spec]`, found `Iterator[Spec | None]`
+ lib/spack/spack/spec_parser.py:501:16: error[invalid-return-type] Return type does not match returned value: expected `list[Spec]`, found `list[Spec | None]`
+ lib/spack/spack/test/cmd/deprecate.py:89:45: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 4294 diagnostics
+ Found 4295 diagnostics

isort (https://github.com/pycqa/isort)
- isort/api.py:636:50: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Iterator[str | Path]`
- Found 33 diagnostics
+ Found 32 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/utils/signal.py:205:12: error[invalid-return-type] Return type does not match returned value: expected `list[tuple[Any, Any]]`, found `list[Unknown | BaseException]`
+ scrapy/utils/signal.py:205:12: error[invalid-return-type] Return type does not match returned value: expected `list[tuple[Any, Any]]`, found `list[Any | BaseException]`

paasta (https://github.com/yelp/paasta)
+ paasta_tools/utils.py:4206:19: warning[redundant-cast] Value is already of type `list[str]`
- Found 1106 diagnostics
+ Found 1107 diagnostics

starlette (https://github.com/encode/starlette)
+ tests/middleware/test_base.py:247:33: error[missing-argument] No argument provided for required parameter 1 of bound method `__init__`
- Found 214 diagnostics
+ Found 215 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/python.py:951:31: error[non-subscriptable] Cannot subscript object of type `_HiddenParam` with no `__getitem__` method
- Found 438 diagnostics
+ Found 439 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- sockeye/inference.py:346:28: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `list[Unknown] | None`
+ sockeye/inference.py:346:28: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `list[str] | None`
- sockeye/inference.py:348:34: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `list[Unknown] | None`
+ sockeye/inference.py:348:34: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `list[str] | None`
- sockeye/inference.py:350:50: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~Top[list[Unknown]]) | None | list[list[Unknown] | Unknown]`
+ sockeye/inference.py:350:50: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~Top[list[Unknown]]) | None | list[list[str] | Unknown]`
- sockeye/inference.py:352:34: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~Top[list[Unknown]]) | None | list[list[Unknown] | Unknown]`
+ sockeye/inference.py:352:34: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~Top[list[Unknown]]) | None | list[list[str] | Unknown]`
+ test/unit/test_inference.py:137:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `list[list[str]] | None`
+ test/unit/test_inference.py:138:48: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[list[str]]`, found `list[list[str]] | None`
+ test/unit/test_inference.py:176:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `list[list[str]] | None`
+ test/unit/test_inference.py:177:79: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[list[str]]`, found `list[list[str]] | None`
- Found 412 diagnostics
+ Found 416 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/pack.py:674:16: error[invalid-return-type] Return type does not match returned value: expected `Iterator[ObjectID]`, found `map[Unknown | bytes]`
+ dulwich/pack.py:1914:16: error[invalid-return-type] Return type does not match returned value: expected `list[tuple[RawObjectID, int, int]]`, found `list[tuple[RawObjectID, int, int | None]]`
+ dulwich/pack.py:1915:83: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/pack.py:3052:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 228 diagnostics
+ Found 230 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
+ dedupe/labeler.py:71:13: error[invalid-assignment] Object of type `list[int]` is not assignable to `Iterable[Literal[0, 1]]`
+ dedupe/labeler.py:72:30: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Iterable[Literal[0, 1]]`
+ dedupe/labeler.py:74:78: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Iterable[Literal[0, 1]]`
+ dedupe/labeler.py:76:16: error[invalid-return-type] Return type does not match returned value: expected `list[Literal[0, 1]]`, found `Iterable[Literal[0, 1]]`
- Found 52 diagnostics
+ Found 56 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/repositories/installed_repository.py:268:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/poetry/repositories/installed_repository.py:275:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/installation/test_executor.py:790:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/installation/test_executor.py:795:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 968 diagnostics
+ Found 964 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ test/mitmproxy/io/test_compat.py:24:16: error[unresolved-attribute] Object of type `Flow` has no attribute `request`
+ test/mitmproxy/test_flow.py:52:16: error[unresolved-attribute] Object of type `Flow` has no attribute `request`
- Found 2142 diagnostics
+ Found 2144 diagnostics

antidote (https://github.com/Finistere/antidote)
- tests/lib/interface/test_lazy.py:440:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_lazy.py:446:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 272 diagnostics
+ Found 270 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_server_cursor_base.py:183:51: error[invalid-argument-type] Argument to bound method `load_rows` is incorrect: Expected `RowMaker[tuple[Any, ...]]`, found `RowMaker[Row@ServerCursorMixin]`
- Found 687 diagnostics
+ Found 686 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/reloaders.py:361:76: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `Generator[Path, None, None]`
- Found 150 diagnostics
+ Found 149 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_file_io.py:269:22: error[invalid-argument-type] Argument to bound method `readline` is incorrect: Argument type `AnyStr@__anext__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 492 diagnostics
+ Found 493 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/help.py:673:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/help.py:677:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ui/modal.py:200:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 565 diagnostics
+ Found 562 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/catalog/marc/tests/test_marc_binary.py:27:27: warning[possibly-missing-attribute] Attribute `get_all_subfields` may be missing on object of type `str | BinaryDataField`
+ openlibrary/catalog/marc/tests/test_marc_binary.py:29:27: warning[possibly-missing-attribute] Attribute `get_all_subfields` may be missing on object of type `str | BinaryDataField`
- Found 1152 diagnostics
+ Found 1154 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/backend/ninjabackend.py:1154:65: error[invalid-argument-type] Argument is incorrect: Expected `list[tuple[str, Literal["cpp", "fortran"]]]`, found `list[tuple[str, str]]`
+ mesonbuild/dependencies/cuda.py:136:76: error[invalid-argument-type] Argument to function `version_compare_many` is incorrect: Expected `str`, found `Unknown | str | None`
- Found 1945 diagnostics
+ Found 1947 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/archive_util.py:288:25: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | PathLike[str] | Unknown`
+ setuptools/_distutils/archive_util.py:288:25: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | PathLike[str]`
- setuptools/_distutils/archive_util.py:288:25: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | PathLike[str] | Unknown`
+ setuptools/_distutils/archive_util.py:288:25: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | PathLike[str]`
- setuptools/_distutils/compilers/C/base.py:1084:41: error[unsupported-operator] Operator `+` is not supported between objects of type `str | PathLike[str] | Unknown` and `str | None`
+ setuptools/_distutils/compilers/C/base.py:1084:41: error[unsupported-operator] Operator `+` is not supported between objects of type `str | PathLike[str]` and `str | None`
- setuptools/_distutils/compilers/C/base.py:1109:41: error[unsupported-operator] Operator `+` is not supported between objects of type `str | PathLike[str] | Unknown` and `(str & ~AlwaysFalsy) | Literal[""]`
+ setuptools/_distutils/compilers/C/base.py:1109:41: error[unsupported-operator] Operator `+` is not supported between objects of type `str | PathLike[str]` and `(str & ~AlwaysFalsy) | Literal[""]`
- setuptools/command/editable_wheel.py:174:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Generator[Path, None, None]`
- setuptools/dist.py:400:48: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Iterator[Requirement]`
- setuptools/dist.py:404:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Iterator[Requirement]`
- Found 1275 diagnostics
+ Found 1272 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/objc.py:1143:22: warning[possibly-missing-attribute] Attribute `name_to_human_readable` may be missing on object of type `Type | None`
- Found 2520 diagnostics
+ Found 2521 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ tests/test_provenance.py:632:47: error[unresolved-attribute] Object of type `Node` has no attribute `endswith`
+ tests/test_provenance.py:638:41: error[invalid-argument-type] Argument to function `_arcp2file` is incorrect: Expected `str`, found `Node | Unknown`
+ tests/test_provenance.py:639:43: error[invalid-argument-type] Argument to bound method `parse` is incorrect: Expected `str | None`, found `Node | Unknown`
- Found 245 diagnostics
+ Found 248 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/core/dataset.py:5379:77: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `set[Hashable] & ~AlwaysFalsy`
- Found 1779 diagnostics
+ Found 1780 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/server/events/storage/__init__.py:88:42: error[not-iterable] Object of type `int` is not iterable
- Found 5536 diagnostics
+ Found 5537 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- tests/test_cmake_ast.py:47:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_cmake_ast.py:48:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 43 diagnostics
+ Found 40 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/externals/array_api_compat/common/_helpers.py:917:11: error[no-matching-overload] No overload of function `prod` matches arguments
- Found 2427 diagnostics
+ Found 2428 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/internal/coverage/instrumentation_py3_12.py:241:21: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown & ~None` and value of type `tuple[None | str, tuple[Unknown, ...]]` on object of type `dict[int, tuple[str, tuple[str, ...]]]`
+ ddtrace/internal/coverage/instrumentation_py3_12.py:241:21: error[invalid-assignment] Invalid subscript assignment with key of type `int` and value of type `tuple[None | str, tuple[Unknown, ...]]` on object of type `dict[int, tuple[str, tuple[str, ...]]]`
- ddtrace/internal/coverage/instrumentation_py3_12.py:246:21: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown & ~None` and value of type `tuple[None | str, tuple[str]]` on object of type `dict[int, tuple[str, tuple[str, ...]]]`
+ ddtrace/internal/coverage/instrumentation_py3_12.py:246:21: error[invalid-assignment] Invalid subscript assignment with key of type `int` and value of type `tuple[None | str, tuple[str]]` on object of type `dict[int, tuple[str, tuple[str, ...]]]`
- tests/testing/internal/pytest/test_pytest_efd.py:70:23: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to `set[TestRef | SuiteRef]`
+ tests/testing/internal/pytest/test_pytest_efd.py:70:23: error[invalid-assignment] Object of type `list[dict[str, Any]]` is not assignable to `set[TestRef | SuiteRef]`

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/layouts.py:298:20: error[invalid-assignment] Object of type `list[Sequence[Unknown]]` is not assignable to `list[UIElement | None] | list[list[UIElement | None]]`
- Found 895 diagnostics
+ Found 896 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/pallas/mosaic/interpret/interpret_pallas_call.py:1890:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/profiler.py:159:17: error[unresolved-attribute] Object of type `int | str | bytes | PathLike[str] | PathLike[bytes]` has no attribute `glob`
+ jax/_src/profiler.py:159:17: error[unresolved-attribute] Object of type `PathLike[str] | int | str | bytes | PathLike[bytes]` has no attribute `glob`
- jax/_src/profiler.py:173:20: error[unsupported-operator] Operator `/` is not supported between objects of type `int | str | bytes | PathLike[str] | PathLike[bytes]` and `Literal["perfetto_trace.json.gz"]`
+ jax/_src/profiler.py:173:20: error[unsupported-operator] Operator `/` is not supported between objects of type `PathLike[str] | int | str | bytes | PathLike[bytes]` and `Literal["perfetto_trace.json.gz"]`
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2805 diagnostics
+ Found 2803 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/backends/sql/datatypes.py:1396:40: error[invalid-assignment] Object of type `dict[Unknown, type[Unknown] | Unknown]` is not assignable to `dict[str, SqlglotType]`
- ibis/expr/api.py:256:39: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Iterable[str] | None`
+ ibis/expr/api.py:256:39: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str]`, found `Iterable[str] | None`
- ibis/expr/api.py:256:46: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Iterable[str | DataType] | None`
+ ibis/expr/api.py:256:46: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str | DataType]`, found `Iterable[str | DataType] | None`
- ibis/expr/datatypes/core.py:968:20: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `dict[Unknown, Unknown]`
+ ibis/expr/datatypes/core.py:968:20: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `dict[str, str | DataType]`
- ibis/expr/types/relations.py:4448:13: warning[possibly-missing-attribute] Attribute `setdefault` may be missing on object of type `(((str, /) -> Value) & Top[Mapping[Unknown, object]]) | Mapping[str, (str, /) -> Value] | Unknown | dict[Unknown, ((str, /) -> Value) & (() -> object) & ~Top[Mapping[Unknown, object]]]`
+ ibis/expr/types/relations.py:4448:13: warning[possibly-missing-attribute] Attribute `setdefault` may be missing on object of type `(((str, /) -> Value) & Top[Mapping[Unknown, object]]) | Mapping[str, (str, /) -> Value] | Unknown`
- Found 4607 diagnostics
+ Found 4608 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/frame.py:1318:29: error[call-non-callable] Object of type `object` is not callable
+ static_frame/core/frame.py:1324:31: error[no-matching-overload] No overload of bound method `astype` matches arguments
+ static_frame/core/frame.py:1454:29: error[call-non-callable] Object of type `object` is not callable
+ static_frame/core/frame.py:1460:31: error[no-matching-overload] No overload of bound method `astype` matches arguments
- static_frame/core/index_correspondence.py:90:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/index_hierarchy_set_utils.py:97:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/reduce.py:243:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/test/unit/test_bus.py:2137:26: warning[possibly-missing-attribute] Attribute `to_pairs` may be missing on object of type `Series[Any, Any] | @Todo | ndarray[Any, Any]`
+ static_frame/test/unit/test_bus.py:2138:26: warning[possibly-missing-attribute] Attribute `to_pairs` may be missing on object of type `Series[Any, Any] | @Todo | ndarray[Any, Any]`
+ static_frame/test/unit/test_bus.py:2166:26: warning[possibly-missing-attribute] Attribute `to_pairs` may be missing on object of type `Series[Any, Any] | @Todo | ndarray[Any, Any]`
+ static_frame/test/unit/test_bus.py:2167:26: warning[possibly-missing-attribute] Attribute `to_pairs` may be missing on object of type `Series[Any, Any] | @Todo | ndarray[Any, Any]`
- Found 1844 diagnostics
+ Found 1849 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/actions/streams.py:769:16: error[unresolved-attribute] Object of type `UserProfile` has no attribute `realm_id`
+ zerver/actions/streams.py:1082:16: error[unresolved-attribute] Object of type `Stream` has no attribute `realm_id`
+ zerver/actions/streams.py:1085:16: error[unresolved-attribute] Object of type `UserProfile` has no attribute `realm_id`
+ zerver/actions/streams.py:1087:20: error[unresolved-attribute] Object of type `Stream` has no attribute `id`
+ zerver/actions/streams.py:1092:23: error[unresolved-attribute] Object of type `Stream` has no attribute `id`
+ zerver/lib/export.py:2319:26: error[unresolved-attribute] Object of type `Attachment` has no attribute `realm_id`
- Found 3652 diagnostics
+ Found 3658 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/_lib/array_api_compat/array_api_compat/common/_helpers.py:927:11: error[no-matching-overload] No overload of function `prod` matches arguments
- Found 7937 diagnostics
+ Found 7938 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-20 21:53_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-20 21:53_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-20 21:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:1869 on 2025-12-20 21:57_

I suggest we add a similar comment to https://github.com/astral-sh/ruff/blob/ad41728204681a60e6d9761857b000cb6bfe732b/crates/ty_python_semantic/src/types/generics.rs#L1900-L1903

```suggestion
			// TODO: This will only handle protocol classes that explicit inherit
			// from other generic protocol classes by listing it as a base class.
			// To handle classes that implicitly implement a generic protocol, we
			// will need to check the types of the protocol members to be able to
			// infer the specialization of the protocol that the class implements. 
            (formal, Type::ProtocolInstance(actual_protocol)) => {
```

---

_@AlexWaygood reviewed on 2025-12-20 21:57_

Nice

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 21:59_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 16 | 8 | 9 |
| `unused-ignore-comment` | 2 | 15 | 0 |
| `invalid-return-type` | 4 | 0 | 7 |
| `unresolved-attribute` | 9 | 0 | 1 |
| `possibly-missing-attribute` | 7 | 0 | 1 |
| `invalid-assignment` | 3 | 0 | 3 |
| `no-matching-overload` | 4 | 0 | 0 |
| `unsupported-operator` | 0 | 0 | 3 |
| `call-non-callable` | 2 | 0 | 0 |
| `missing-argument` | 1 | 0 | 0 |
| `non-subscriptable` | 1 | 0 | 0 |
| `not-iterable` | 1 | 0 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| **Total** | **51** | **23** | **24** |


**[Full report with detailed diff](https://98cdbccf.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://98cdbccf.ty-ecosystem-ext.pages.dev/timing))



---

_@carljm approved on 2025-12-23 01:42_

---

_Comment by @MichaReiser on 2025-12-23 07:29_

I'll go ahead and merge this, given that @ibraheemdev is out for the rest of the week.

---

_Comment by @MichaReiser on 2025-12-23 07:41_

I'm very confused why the benchmark job fails here when it succeeded on https://github.com/astral-sh/ruff/actions/runs/20426087014/job/58686630346

---

_Closed by @MichaReiser on 2025-12-23 08:12_

---

_Reopened by @MichaReiser on 2025-12-23 08:12_

---

_Merged by @MichaReiser on 2025-12-23 08:24_

---

_Closed by @MichaReiser on 2025-12-23 08:24_

---

_Branch deleted on 2025-12-23 08:24_

---
