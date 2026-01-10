```yaml
number: 21930
title: "[ty] Subtyping for bidirectional inference"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/bidi-subtyping2
created_at: 2025-12-11T20:34:44Z
updated_at: 2025-12-30T22:03:21Z
url: https://github.com/astral-sh/ruff/pull/21930
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Subtyping for bidirectional inference

---

_Pull request opened by @ibraheemdev on 2025-12-11 20:34_

## Summary

Supersedes https://github.com/astral-sh/ruff/pull/21747. This version uses the constraint solver directly, which means we should benefit from constraint solver improvements for free.

Resolves https://github.com/astral-sh/ty/issues/1576.

---

_Label `ty` added by @ibraheemdev on 2025-12-11 20:34_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 20:39_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-30 21:45:19.892689916 +0000
+++ new-output.txt	2025-12-30 21:45:20.209690328 +0000
@@ -805,7 +805,7 @@
 overloads_evaluation.py:291:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@example6`
 protocols_class_objects.py:58:16: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA1`
 protocols_class_objects.py:59:16: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA2`
-protocols_definition.py:30:11: error[invalid-argument-type] Argument to function `close_all` is incorrect: Expected `Iterable[SupportsClose]`, found `list[Unknown | int]`
+protocols_definition.py:30:11: error[invalid-argument-type] Argument to function `close_all` is incorrect: Expected `Iterable[SupportsClose]`, found `list[int]`
 protocols_definition.py:114:22: error[invalid-assignment] Object of type `Concrete2_Bad1` is not assignable to `Template2`
 protocols_definition.py:115:22: error[invalid-assignment] Object of type `Concrete2_Bad2` is not assignable to `Template2`
 protocols_definition.py:116:22: error[invalid-assignment] Object of type `Concrete2_Bad3` is not assignable to `Template2`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-11 20:41_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paroxython (https://github.com/laowantong/paroxython)
+ paroxython/derived_labels_db.py:189:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[LabelName, dict[Span, Any]].__getitem__(key: LabelName, /) -> dict[Span, Any]` cannot be called with key of type `str | Unknown` on object of type `defaultdict[LabelName, dict[Span, Any]]`
- Found 7 diagnostics
+ Found 8 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/test/cmd/checksum.py:251:9: error[invalid-argument-type] Argument to function `interactive_version_filter` is incorrect: Expected `Iterable[StandardVersion]`, found `list[Unknown | StandardVersion | GitVersion]`
+ lib/spack/spack/test/cmd/checksum.py:251:9: error[invalid-argument-type] Argument to function `interactive_version_filter` is incorrect: Expected `Iterable[StandardVersion]`, found `list[StandardVersion | GitVersion]`
- lib/spack/spack/vendor/jinja2/environment.py:1114:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[Unknown, Unknown]`, found `Mapping[str, Any] | dict[Unknown, Unknown]`
+ lib/spack/spack/vendor/jinja2/environment.py:1114:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, Any]`, found `Mapping[str, Any]`

jinja (https://github.com/pallets/jinja)
- tests/test_loader.py:293:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, BaseLoader]`, found `dict[Unknown | str, Unknown | BaseLoader | None]`
+ tests/test_loader.py:293:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, BaseLoader]`, found `dict[str, Unknown | BaseLoader | None]`

pip (https://github.com/pypa/pip)
+ src/pip/_internal/commands/search.py:101:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[Unknown | str, Unknown | str | list[Unknown | str]]` on object of type `OrderedDict[str, TransformedHit]`
- Found 612 diagnostics
+ Found 613 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/wsgi.py:244:25: error[invalid-assignment] Object of type `list[Unknown | (() -> None) | (Iterable[() -> None] & Top[(...) -> object])]` is not assignable to `None | (() -> None) | Iterable[() -> None]`
+ src/werkzeug/wsgi.py:244:25: error[invalid-assignment] Object of type `list[(() -> None) | (Iterable[() -> None] & Top[(...) -> object])]` is not assignable to `None | (() -> None) | Iterable[() -> None]`
- tests/test_routing.py:615:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Rule]`, found `list[Unknown | Submount | EndpointPrefix | Subdomain]`
+ tests/test_routing.py:615:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Rule]`, found `list[Submount | EndpointPrefix | Subdomain]`
- tests/test_send_file.py:131:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
+ tests/test_send_file.py:131:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[str, Unknown | str]`
- tests/test_test.py:223:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
+ tests/test_test.py:223:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[str, Unknown | str]`
- tests/test_test.py:227:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
+ tests/test_test.py:227:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[str, Unknown | str]`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:1676:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
+ src/graphql/execution/execute.py:1676:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult | CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
- src/graphql/execution/execute.py:1828:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[CoroutineType[Any, Any, StreamItemResult] | StreamItemResult]`
+ src/graphql/execution/execute.py:1828:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[StreamItemResult | CoroutineType[Any, Any, StreamItemResult]]`

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/python.py:953:45: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `str | _HiddenParam` on object of type `defaultdict[str, int]`
+ src/_pytest/python.py:955:25: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `str | _HiddenParam` on object of type `defaultdict[str, int]`
+ src/_pytest/python.py:956:49: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `str | _HiddenParam` on object of type `defaultdict[str, int]`
+ src/_pytest/python.py:958:21: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `str | _HiddenParam` on object of type `defaultdict[str, int]`
- Found 429 diagnostics
+ Found 433 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/contrib/service_shard_update.py:283:25: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | str`
- paasta_tools/instance/kubernetes.py:381:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | str | int | list[Unknown]`
- paasta_tools/kubernetes_tools.py:3177:17: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `Unknown | list[Unknown] | str`
- paasta_tools/utils.py:609:17: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
+ paasta_tools/utils.py:609:17: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[dict[Unknown | str, Unknown | str]]`
- paasta_tools/utils.py:617:35: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
+ paasta_tools/utils.py:617:35: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[dict[Unknown | str, Unknown | str]]`
- paasta_tools/utils.py:630:16: error[invalid-return-type] Return type does not match returned value: expected `Iterable[DockerParameter]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
+ paasta_tools/utils.py:630:16: error[invalid-return-type] Return type does not match returned value: expected `Iterable[DockerParameter]`, found `list[dict[Unknown | str, Unknown | str]]`
- Found 1107 diagnostics
+ Found 1106 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_contracts.py:571:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[Unknown | CustomFailContractPreProcess]`
+ tests/test_contracts.py:571:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[CustomFailContractPreProcess]`
- tests/test_contracts.py:585:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[Unknown | CustomFailContractPostProcess]`
+ tests/test_contracts.py:585:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[CustomFailContractPostProcess]`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/contrib/swift.py:161:35: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[Unknown | tuple[Unknown | bytes, None, None, bool]]`
+ dulwich/contrib/swift.py:161:35: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[tuple[Unknown | bytes, None, None, bool]]`
- dulwich/contrib/swift.py:921:13: error[invalid-argument-type] Argument to bound method `add_objects` is incorrect: Expected `Sequence[tuple[ShaFile, str | None]]`, found `list[Unknown | tuple[object, None]]`
+ dulwich/contrib/swift.py:921:13: error[invalid-argument-type] Argument to bound method `add_objects` is incorrect: Expected `Sequence[tuple[ShaFile, str | None]]`, found `list[tuple[object, None]]`
- dulwich/object_store.py:2901:31: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[Unknown | tuple[Unknown | bytes, None, Unknown | int, bool]]`
+ dulwich/object_store.py:2901:31: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[tuple[Unknown | bytes, None, Unknown | int, bool]]`
- dulwich/walk.py:332:23: error[invalid-assignment] Object of type `list[Unknown | ObjectID | (Sequence[ObjectID] & bytes)]` is not assignable to `ObjectID | Sequence[ObjectID]`
+ dulwich/walk.py:332:23: error[invalid-assignment] Object of type `list[ObjectID | (Sequence[ObjectID] & bytes)]` is not assignable to `ObjectID | Sequence[ObjectID]`

dedupe (https://github.com/dedupeio/dedupe)
- dedupe/labeler.py:71:13: error[invalid-assignment] Object of type `list[int]` is not assignable to `Iterable[Literal[0, 1]]`
- dedupe/labeler.py:72:30: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Iterable[Literal[0, 1]]`
- dedupe/labeler.py:74:78: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Iterable[Literal[0, 1]]`
- dedupe/labeler.py:76:16: error[invalid-return-type] Return type does not match returned value: expected `list[Literal[0, 1]]`, found `Iterable[Literal[0, 1]]`
- Found 56 diagnostics
+ Found 52 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/metrics/nlp/test_bleu.py:26:29: error[invalid-argument-type] Argument to bound method `_corpus_bleu` is incorrect: Expected `Sequence[Sequence[Sequence[Any]]]`, found `list[Unknown | list[Unknown | int]]`
+ tests/ignite/metrics/nlp/test_bleu.py:26:29: error[invalid-argument-type] Argument to bound method `_corpus_bleu` is incorrect: Expected `Sequence[Sequence[Sequence[Any]]]`, found `list[list[int]]`

mkosi (https://github.com/systemd/mkosi)
- mkosi/__init__.py:2579:17: error[invalid-argument-type] Argument to bound method `sandbox` is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | str | None | Path]`
+ mkosi/__init__.py:2579:17: error[invalid-argument-type] Argument to bound method `sandbox` is incorrect: Expected `Sequence[Path | str]`, found `list[str | Unknown | None | Path]`
- mkosi/__init__.py:4415:9: error[invalid-argument-type] Argument to function `run` is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | Path | None | str]`
+ mkosi/__init__.py:4415:9: error[invalid-argument-type] Argument to function `run` is incorrect: Expected `Sequence[Path | str]`, found `list[Path | None | str | @Todo(StarredExpression)]`
- mkosi/qemu.py:527:13: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | Path | None | str]`
+ mkosi/qemu.py:527:13: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[Path | str]`, found `list[Path | None | str]`

PyGithub (https://github.com/PyGithub/PyGithub)
- github/RepositoryAdvisory.py:153:13: error[invalid-argument-type] Argument to bound method `add_vulnerabilities` is incorrect: Expected `Iterable[SimpleAdvisoryVulnerability | AdvisoryVulnerability]`, found `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str | None] | str | None | list[str]]]`
- github/RepositoryAdvisory.py:199:28: error[invalid-argument-type] Argument to bound method `offer_credits` is incorrect: Expected `Iterable[SimpleCredit | AdvisoryCredit]`, found `list[Unknown | dict[Unknown | str, Unknown | str | NamedUser]]`
- tests/RepositoryAdvisory.py:141:13: error[invalid-argument-type] Argument to bound method `offer_credits` is incorrect: Expected `Iterable[SimpleCredit | AdvisoryCredit]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
- Found 301 diagnostics
+ Found 298 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`

nox (https://github.com/wntrblm/nox)
- nox/_option_set.py:63:66: error[invalid-assignment] Object of type `dataclasses.Field[Unknown | str | None]` is not assignable to `None | Literal["auto", "never", "always"]`
- Found 25 diagnostics
+ Found 24 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_generate_schema.py:2472:5: error[invalid-assignment] Object of type `dict[Unknown | tuple[str, str], Unknown | ((f, schema) -> Unknown) | ((f, _) -> Unknown)]` is not assignable to `Mapping[tuple[Literal["before", "after", "wrap", "plain"], Literal["no-info", "with-info"]], ((...) -> Any, InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements, /) -> InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements]`
- pydantic/v1/utils.py:613:16: error[invalid-return-type] Return type does not match returned value: expected `Mapping[int | str, Any]`, found `AbstractSet[int | str] | Mapping[int | str, Any] | dict[Unknown, ellipsis]`
+ pydantic/v1/utils.py:613:16: error[invalid-return-type] Return type does not match returned value: expected `Mapping[int | str, Any]`, found `AbstractSet[int | str] | Mapping[int | str, Any] | dict[int | str, ellipsis]`
- Found 3160 diagnostics
+ Found 3159 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/ops/poolers.py:283:34: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `tuple[int] | list[int] | int`
+ torchvision/ops/poolers.py:283:34: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[Unknown]`, found `tuple[int] | list[int] | int`

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/pandas/array.py:23:22: error[not-iterable] Object of type `Unknown | (list[Check] & ~Check) | (list[Unknown] & ~Check) | list[Unknown | Check] | None` may not be iterable
+ pandera/api/pandas/array.py:23:22: error[not-iterable] Object of type `Unknown | list[Check] | None` may not be iterable
+ pandera/backends/polars/base.py:143:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Unknown | None` on object of type `defaultdict[str, int]`
- tests/pandas/test_schemas.py:620:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | (list[Check] & ~Check) | (list[Unknown] & ~Check) | list[Unknown | Check] | None`
+ tests/pandas/test_schemas.py:620:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | list[Check] | None`
- tests/pandas/test_schemas.py:621:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | (list[Check] & ~Check) | (list[Unknown] & ~Check) | list[Unknown | Check] | None`
+ tests/pandas/test_schemas.py:621:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | list[Check] | None`
- tests/pandas/test_schemas.py:622:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | (list[Check] & ~Check) | (list[Unknown] & ~Check) | list[Unknown | Check] | None`
+ tests/pandas/test_schemas.py:622:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | list[Check] | None`
- Found 1573 diagnostics
+ Found 1574 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/test_http.py:588:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[Unknown | list[Unknown | bytes]]`
+ test/mitmproxy/test_http.py:588:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[list[Unknown | bytes]]`
- test/mitmproxy/test_http.py:821:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[Unknown | tuple[bytes, str]]`
+ test/mitmproxy/test_http.py:821:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[tuple[bytes, str]]`

mypy (https://github.com/python/mypy)
- mypyc/irbuild/util.py:35:44: error[invalid-assignment] Object of type `frozenset[Unknown | str]` is not assignable to `frozenset[Literal["native_class", "allow_interpreted_subclasses", "serializable", "free_list_len"]]`
+ mypyc/irbuild/util.py:35:44: error[invalid-assignment] Object of type `frozenset[str]` is not assignable to `frozenset[Literal["native_class", "allow_interpreted_subclasses", "serializable", "free_list_len"]]`

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/dictionary.py:60:20: error[invalid-return-type] Return type does not match returned value: expected `Valid[Just[A@KeyNotRequired] | Nothing] | Invalid`, found `Valid[Just[Unknown | A@KeyNotRequired]]`
- koda_validate/dictionary.py:67:20: error[invalid-return-type] Return type does not match returned value: expected `Valid[Just[A@KeyNotRequired] | Nothing] | Invalid`, found `Valid[Just[Unknown | A@KeyNotRequired]]`
- Found 407 diagnostics
+ Found 405 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/context/menu.py:57:96: error[invalid-assignment] Object of type `frozenset[Unknown | CommandType]` is not assignable to `frozenset[Literal[CommandType.USER, CommandType.MESSAGE]]`
+ tanjun/context/menu.py:57:96: error[invalid-assignment] Object of type `frozenset[CommandType]` is not assignable to `frozenset[Literal[CommandType.USER, CommandType.MESSAGE]]`

build (https://github.com/pypa/build)
- tests/test_main.py:272:58: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:287:58: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:304:58: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:332:62: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:342:62: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:348:68: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:359:78: error[invalid-argument-type] Argument to function `build_package_via_sdist` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:370:92: error[invalid-argument-type] Argument to function `build_package_via_sdist` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:375:82: error[invalid-argument-type] Argument to function `build_package_via_sdist` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- Found 58 diagnostics
+ Found 49 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/coding/cftime_offsets.py:687:54: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | type[BaseCFTimeOffset] | partial[QuarterBegin] | partial[QuarterEnd]]` is not assignable to `Mapping[str, type[BaseCFTimeOffset]]`
+ xarray/coding/cftime_offsets.py:687:54: error[invalid-assignment] Object of type `dict[str, type[BaseCFTimeOffset] | partial[QuarterBegin] | partial[QuarterEnd]]` is not assignable to `Mapping[str, type[BaseCFTimeOffset]]`
+ xarray/core/dataset.py:6196:28: error[unsupported-operator] Operator `-` is not supported between objects of type `set[str] | set[Unknown]` and `set[Hashable]`
- xarray/core/datatree.py:1341:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[tuple[str | Unknown, _CoordWrapper]]`, found `Iterable[tuple[str, Unknown]] | ItemsView[str, DataArray | Variable | Any | ... omitted 9 union elements] | dict_items[Unknown, Unknown]`
+ xarray/core/groupby.py:500:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataArray`, found `T_DataWithCoords@_resolve_group`
- xarray/tests/test_backends.py:3796:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, bool | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`, found `dict[str, bool]`
- xarray/tests/test_backends.py:3796:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, bool | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`, found `dict[str, dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`
- xarray/tests/test_backends.py:3813:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, bool | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`, found `dict[str, bool]`
- xarray/tests/test_backends.py:3813:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, bool | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`, found `dict[str, dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`
- xarray/tests/test_dataarray.py:2700:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Sequence[SequenceNotStr[Hashable]]`, found `list[Unknown | Index[Any]]`
+ xarray/tests/test_dataarray.py:2700:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Sequence[SequenceNotStr[Hashable]]`, found `list[Index[Any]]`
- Found 1768 diagnostics
+ Found 1765 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: @Todo, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[CoroutineType[Any, Any, None]]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
+ pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[str, ((credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: @Todo, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[CoroutineType[Any, Any, None]]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
- pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: Connection) -> None) | ((credentials: @Todo, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[None]]` is not assignable to `Mapping[str, (...) -> None]`
+ pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[str, ((credentials: @Todo, conn: Connection) -> None) | ((credentials: @Todo, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[None]]` is not assignable to `Mapping[str, (...) -> None]`

discord.py (https://github.com/Rapptz/discord.py)
- discord/interactions.py:229:58: error[invalid-argument-type] Argument to bound method `_from_value` is incorrect: Expected `Sequence[Literal[0, 1, 2]]`, found `list[Unknown | int]`
- Found 553 diagnostics
+ Found 552 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/cwlviewer.py:56:40: error[invalid-argument-type] Argument to bound method `query` is incorrect: Expected `Mapping[str, Identifier] | None`, found `dict[Unknown | str, Unknown | str]`
+ cwltool/cwlviewer.py:56:40: error[invalid-argument-type] Argument to bound method `query` is incorrect: Expected `Mapping[str, Identifier] | None`, found `dict[str, str]`
- cwltool/cwlviewer.py:124:40: error[invalid-argument-type] Argument to bound method `query` is incorrect: Expected `Mapping[str, Identifier] | None`, found `dict[Unknown | str, Unknown | str]`
+ cwltool/cwlviewer.py:124:40: error[invalid-argument-type] Argument to bound method `query` is incorrect: Expected `Mapping[str, Identifier] | None`, found `dict[str, str]`
- cwltool/cwlviewer.py:155:35: error[invalid-argument-type] Argument to bound method `query` is incorrect: Expected `Mapping[str, Identifier] | None`, found `dict[Unknown | str, Unknown | str]`
+ cwltool/cwlviewer.py:155:35: error[invalid-argument-type] Argument to bound method `query` is incorrect: Expected `Mapping[str, Identifier] | None`, found `dict[str, str]`
- cwltool/main.py:301:42: error[invalid-argument-type] Argument to function `realize_input_schema` is incorrect: Expected `MutableSequence[str | MutableMapping[str, None | int | str | ... omitted 3 union elements]]`, found `list[Unknown | (int & Top[Mapping[Unknown, object]]) | (str & Top[Mapping[Unknown, object]]) | ... omitted 3 union elements]`
+ cwltool/main.py:301:42: error[invalid-argument-type] Argument to function `realize_input_schema` is incorrect: Expected `MutableSequence[str | MutableMapping[str, None | int | str | ... omitted 3 union elements]]`, found `list[str | MutableMapping[str, None | int | str | ... omitted 3 union elements] | (@Todo & Top[Mapping[Unknown, object]]) | ... omitted 4 union elements]`
- cwltool/main.py:477:33: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
+ cwltool/main.py:477:33: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None`
- cwltool/main.py:499:37: error[invalid-argument-type] Argument to bound method `_init_job` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
+ cwltool/main.py:499:37: error[invalid-argument-type] Argument to bound method `_init_job` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None`
- cwltool/main.py:502:43: error[invalid-argument-type] Argument to bound method `bind_input` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements] | list[MutableMapping[str, None | int | str | ... omitted 3 union elements]]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
+ cwltool/main.py:502:43: error[invalid-argument-type] Argument to bound method `bind_input` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements] | list[MutableMapping[str, None | int | str | ... omitted 3 union elements]]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None`
- cwltool/main.py:510:13: error[invalid-argument-type] Argument to function `printdeps` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
+ cwltool/main.py:510:13: error[invalid-argument-type] Argument to function `printdeps` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None`
- cwltool/main.py:523:13: error[invalid-argument-type] Argument to bound method `store` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
+ cwltool/main.py:523:13: error[invalid-argument-type] Argument to bound method `store` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None`
- cwltool/main.py:526:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["cwl:tool"]` and `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
+ cwltool/main.py:526:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["cwl:tool"]` and `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None`
- cwltool/main.py:528:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["__id"]` and `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
+ cwltool/main.py:528:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["__id"]` and `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None`
- cwltool/main.py:530:12: error[invalid-return-type] Return type does not match returned value: expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
+ cwltool/main.py:530:12: error[invalid-return-type] Return type does not match returned value: expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None`

apprise (https://github.com/caronc/apprise)
- apprise/plugins/fortysixelks.py:161:23: error[invalid-assignment] Object of type `list[Unknown | (str & ~Literal[""]) | None]` is not assignable to `Iterable[str] | None`
+ apprise/plugins/fortysixelks.py:161:23: error[invalid-assignment] Object of type `list[(str & ~Literal[""]) | None]` is not assignable to `Iterable[str] | None`

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/sources/helpers/aliyun.py:174:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- cloudinit/sources/helpers/aliyun.py:202:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- cloudinit/sources/helpers/aliyun.py:205:12: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | dict[Unknown, Unknown]`
- cloudinit/sources/helpers/aliyun.py:206:25: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Unknown | int | dict[Unknown, Unknown]`
- cloudinit/sources/helpers/aliyun.py:207:13: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- cloudinit/sources/helpers/aliyun.py:208:13: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- cloudinit/sources/helpers/aliyun.py:209:13: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- cloudinit/sources/helpers/aliyun.py:210:13: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `str` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `LiteralString` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `str` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `LiteralString` on object of type `list[Unknown | str]`
- Found 1196 diagnostics
+ Found 1188 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/__init__.py:88:46: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `<class 'IInterfaceDeclaration'>`
+ src/zope/interface/__init__.py:88:46: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[Unknown]`, found `<class 'IInterfaceDeclaration'>`

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_tests/test_run.py:2853:22: error[invalid-assignment] Object of type `ExceptionGroup[Exception]` is not assignable to `ValueError | ExceptionGroup[ValueError]`
- Found 490 diagnostics
+ Found 491 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/backend/ninjabackend.py:1692:37: error[unresolved-attribute] Object of type `File` has no attribute `name`
+ mesonbuild/backend/ninjabackend.py:1695:17: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown` and value of type `CustomTarget | CustomTargetIndex | GeneratedList` on object of type `OrderedDict[str, File]`
+ mesonbuild/mconf.py:290:17: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]].__getitem__(key: str, /) -> dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]` cannot be called with key of type `Unknown | str | None` on object of type `defaultdict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]]`
- mesonbuild/modules/__init__.py:81:52: error[invalid-argument-type] Argument to bound method `find_program_impl` is incorrect: Expected `list[File | str]`, found `(File & Top[list[Unknown]]) | list[File | str] | list[Unknown | (File & ~Top[list[Unknown]]) | str]`
+ mesonbuild/modules/__init__.py:81:52: error[invalid-argument-type] Argument to bound method `find_program_impl` is incorrect: Expected `list[File | str]`, found `(File & Top[list[Unknown]]) | list[File | str]`
- mesonbuild/modules/codegen.py:317:26: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Literal["lex", "flex", "reflex", "win_flex"]]`, found `list[Unknown | str]`
- mesonbuild/modules/codegen.py:398:55: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | list[str]]` is not assignable to `Mapping[Literal["yacc", "byacc", "bison", "win_bison"], list[str]]`
- mesonbuild/modules/i18n.py:425:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[str | BuildTarget | CustomTarget | ... omitted 4 union elements]`, found `list[Unknown | ExternalProgram | Executable | None | str]`
+ mesonbuild/modules/i18n.py:425:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[str | BuildTarget | CustomTarget | ... omitted 4 union elements]`, found `list[ExternalProgram | Executable | None | str]`
- mesonbuild/mparser.py:681:47: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `Mapping[str, Literal["==", "!=", "<", "<=", ">=", ... omitted 3 literals]]`
- mesonbuild/mparser.py:692:47: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `Mapping[str, Literal["+", "-", "*", "/", "%"]]`
- mesonbuild/mparser.py:697:47: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `Mapping[str, Literal["+", "-", "*", "/", "%"]]`
- Found 1946 diagnostics
+ Found 1944 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/federation/schema.py:83:17: error[invalid-assignment] Object of type `list[Unknown | <NewType pseudo-class 'FederationAny'>]` is not assignable to `Iterable[type]`
+ strawberry/federation/schema.py:83:17: error[invalid-assignment] Object of type `list[@Todo(StarredExpression) | <NewType pseudo-class 'FederationAny'>]` is not assignable to `Iterable[type]`

pycryptodome (https://github.com/Legrandin/pycryptodome)
- lib/Crypto/PublicKey/RSA.py:368:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | DerObject] | None`, found `list[Unknown | int | IntegerBase]`
+ lib/Crypto/PublicKey/RSA.py:368:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | DerObject] | None`, found `list[int | Unknown | IntegerBase]`
- lib/Crypto/SelfTest/Util/test_asn1.py:329:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | DerObject] | None`, found `list[Unknown | int | DerInteger | bytes]`
+ lib/Crypto/SelfTest/Util/test_asn1.py:329:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | DerObject] | None`, found `list[int | DerInteger | bytes]`
- lib/Crypto/Signature/pkcs1_15.py:192:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | DerObject] | None`, found `list[Unknown | bytes]`
+ lib/Crypto/Signature/pkcs1_15.py:192:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | DerObject] | None`, found `list[bytes]`
- lib/Crypto/Signature/pkcs1_15.py:198:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | DerObject] | None`, found `list[Unknown | bytes]`
+ lib/Crypto/Signature/pkcs1_15.py:198:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | DerObject] | None`, found `list[bytes]`

scipy-stubs (https://github.com/scipy/scipy-stubs)
- tests/sparse/test_construct.pyi:40:1: error[type-assertion-failure] Type `dia_array[float64] | dia_array[complex128]` does not match asserted type `Unknown`
- tests/sparse/test_construct.pyi:41:1: error[type-assertion-failure] Type `dia_array[float64] | dia_array[complex128]` does not match asserted type `Unknown`
- tests/sparse/test_construct.pyi:42:1: error[type-assertion-failure] Type `dia_array[float64] | dia_array[complex128]` does not match asserted type `Unknown`
- tests/sparse/test_construct.pyi:43:1: error[type-assertion-failure] Type `dia_array[float64] | dia_array[complex128]` does not match asserted type `Unknown`
- tests/sparse/test_construct.pyi:44:1: error[type-assertion-failure] Type `dia_array[float64] | dia_array[complex128]` does not match asserted type `Unknown`
- tests/sparse/test_construct.pyi:45:1: error[type-assertion-failure] Type `dia_array[float64] | dia_array[complex128]` does not match asserted type `Unknown`
- tests/sparse/test_construct.pyi:252:1: error[type-assertion-failure] Type `bsr_array[floating[_32Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:252:1: error[type-assertion-failure] Type `bsr_array[floating[_32Bit]]` does not match asserted type `bsr_array[Unknown]`
- tests/sparse/test_construct.pyi:253:1: error[type-assertion-failure] Type `bsr_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:253:1: error[type-assertion-failure] Type `bsr_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `bsr_array[numpy.bool[builtins.bool]]`
- tests/sparse/test_construct.pyi:254:1: error[type-assertion-failure] Type `bsr_array[signedinteger[_64Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:254:1: error[type-assertion-failure] Type `bsr_array[signedinteger[_64Bit]]` does not match asserted type `bsr_array[numpy.bool[builtins.bool]]`
- tests/sparse/test_construct.pyi:255:1: error[type-assertion-failure] Type `bsr_array[float64]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:255:1: error[type-assertion-failure] Type `bsr_array[float64]` does not match asserted type `bsr_array[numpy.bool[builtins.bool]]`
- tests/sparse/test_construct.pyi:256:1: error[type-assertion-failure] Type `bsr_array[complex128]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:256:1: error[type-assertion-failure] Type `bsr_array[complex128]` does not match asserted type `bsr_array[numpy.bool[builtins.bool]]`
- tests/sparse/test_construct.pyi:257:1: error[type-assertion-failure] Type `coo_array[complexfloating[_32Bit, _32Bit], tuple[int, int]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:257:1: error[type-assertion-failure] Type `coo_array[complexfloating[_32Bit, _32Bit], tuple[int, int]]` does not match asserted type `coo_array[numpy.bool[builtins.bool], tuple[int, int]]`
- tests/sparse/test_construct.pyi:258:1: error[type-assertion-failure] Type `csc_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:258:1: error[type-assertion-failure] Type `csc_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `csc_array[numpy.bool[builtins.bool]]`
- tests/sparse/test_construct.pyi:259:1: error[type-assertion-failure] Type `csr_array[complexfloating[_32Bit, _32Bit], tuple[int, int]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:259:1: error[type-assertion-failure] Type `csr_array[complexfloating[_32Bit, _32Bit], tuple[int, int]]` does not match asserted type `csr_array[numpy.bool[builtins.bool], tuple[int, int]]`
- tests/sparse/test_construct.pyi:260:1: error[type-assertion-failure] Type `dia_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:260:1: error[type-assertion-failure] Type `dia_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `dia_array[numpy.bool[builtins.bool]]`
- tests/sparse/test_construct.pyi:261:1: error[type-assertion-failure] Type `dok_array[complexfloating[_32Bit, _32Bit], tuple[int, int]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:261:1: error[type-assertion-failure] Type `dok_array[complexfloating[_32Bit, _32Bit], tuple[int, int]]` does not match asserted type `dok_array[numpy.bool[builtins.bool], tuple[int, int]]`
- tests/sparse/test_construct.pyi:262:1: error[type-assertion-failure] Type `lil_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:262:1: error[type-assertion-failure] Type `lil_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `lil_array[numpy.bool[builtins.bool]]`
- tests/sparse/test_construct.pyi:289:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:289:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `coo_array[Unknown, tuple[int, int]]`
- tests/sparse/test_construct.pyi:291:1: error[type-assertion-failure] Type `coo_array[numpy.bool[builtins.bool], tuple[int, int]]` does not match asserted type `Unknown`
- tests/sparse/test_construct.pyi:292:1: error[type-assertion-failure] Type `coo_array[signedinteger[_64Bit], tuple[int, int]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:292:1: error[type-assertion-failure] Type `coo_array[signedinteger[_64Bit], tuple[int, int]]` does not match asserted type `coo_array[numpy.bool[builtins.bool], tuple[int, int]]`
- tests/sparse/test_construct.pyi:297:1: error[type-assertion-failure] Type `bsr_array[floating[_32Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:297:1: error[type-assertion-failure] Type `bsr_array[floating[_32Bit]]` does not match asserted type `bsr_array[Unknown]`
- tests/sparse/test_construct.pyi:298:1: error[type-assertion-failure] Type `csc_array[float64]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:298:1: error[type-assertion-failure] Type `csc_array[float64]` does not match asserted type `csc_array[numpy.bool[builtins.bool]]`
- tests/sparse/test_construct.pyi:299:1: error[type-assertion-failure] Type `csr_array[complex128, tuple[int, int]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:299:1: error[type-assertion-failure] Type `csr_array[complex128, tuple[int, int]]` does not match asserted type `csr_array[numpy.bool[builtins.bool], tuple[int, int]]`
- tests/sparse/test_construct.pyi:300:1: error[type-assertion-failure] Type `dia_array[signedinteger[_64Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:300:1: error[type-assertion-failure] Type `dia_array[signedinteger[_64Bit]]` does not match asserted type `dia_array[numpy.bool[builtins.bool]]`
- tests/sparse/test_construct.pyi:301:1: error[type-assertion-failure] Type `dok_array[numpy.bool[builtins.bool], tuple[int, int]]` does not match asserted type `Unknown`
- tests/sparse/test_construct.pyi:302:1: error[type-assertion-failure] Type `lil_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:302:1: error[type-assertion-failure] Type `lil_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `lil_array[numpy.bool[builtins.bool]]`
- tests/stats/test_mode.pyi:29:1: error[type-assertion-failure] Type `signedinteger[_64Bit]` does not match asserted type `Unknown`
- tests/stats/test_mode.pyi:44:1: error[type-assertion-failure] Type `signedinteger[_64Bit]` does not match asserted type `Unknown`
- Found 1256 diagnostics
+ Found 1246 diagnostics

prefect (https://git

... (truncated 751 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~287MB
+ TOTAL MEMORY USAGE: ~301MB


```

</details>




---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-11 20:43_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 20:52_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 14 | 96 | 200 |
| `type-assertion-failure` | 0 | 128 | 18 |
| `invalid-assignment` | 4 | 21 | 63 |
| `invalid-return-type` | 1 | 7 | 11 |
| `possibly-missing-attribute` | 1 | 7 | 1 |
| `unresolved-attribute` | 9 | 0 | 0 |
| `invalid-await` | 0 | 0 | 6 |
| `no-matching-overload` | 1 | 5 | 0 |
| `not-subscriptable` | 0 | 4 | 0 |
| `unsupported-operator` | 1 | 0 | 3 |
| `unused-ignore-comment` | 1 | 2 | 0 |
| `not-iterable` | 0 | 1 | 1 |
| **Total** | **32** | **271** | **303** |


**[Full report with detailed diff](https://be717ff0.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://be717ff0.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @codspeed-hq[bot] on 2025-12-11 20:57_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-subtyping2?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21930 will **degrade performance by 4.81%**

<sub>Comparing <code>ibraheem/bidi-subtyping2</code> (2cd2a50) with <code>main</code> (4f2529f)</sub>



### Summary

`⚡ 1` improvement  
`❌ 1` regression  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-subtyping2?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-subtyping2?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 98 s | 93.9 s | +4.4% |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-subtyping2?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 20.7 s | 21.8 s | -4.81% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-subtyping2?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-12-11 21:11_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-11 21:11_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-12-18 01:51_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-18 01:51_

---

_Marked ready for review by @ibraheemdev on 2025-12-18 02:09_

---

_Review requested from @carljm by @ibraheemdev on 2025-12-18 02:09_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-18 02:09_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-18 02:09_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-18 02:09_

---

_Comment by @ibraheemdev on 2025-12-18 02:11_

Still going through the ecosystem report, but it looks more promising than https://github.com/astral-sh/ruff/pull/21747.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-12-18 21:37_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-18 21:37_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-20 00:37_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-20 00:38_

---

_Comment by @ibraheemdev on 2025-12-20 00:57_

Ecosystem report looks a lot better now!

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:446 on 2025-12-20 01:28_

I've actually come around to the point of view that the right resolution to https://github.com/astral-sh/ty/issues/136 is to always prefer the declared type in an annotated assignment, and consider it a feature that `x: int | str = 1` has different behavior from `x: int | str` followed by `x = 1`, since that "inconsistency" actually gives the user more control, and is visually intuitive (in the sense that a directly annotated assignment looks more like a "cast" of the RHS, not just setting an upper bound.)

Which means that when we make that change, we'd want to reveal the full union type in both of these cases.

But I think that's kind of unrelated to the point of the test. We'd probably just want to rewrite all these tests to separate the declaration (type context) onto a separate line from the assignment, so we are still checking how type context influences type inference, not just verifying our preference for the declared type.

---

_Comment by @carljm on 2025-12-20 02:35_

I went through all the examples in https://github.com/astral-sh/ty/issues/1576 (and everything closed as a duplicate of it.) Most of them are fixed by this PR. https://github.com/astral-sh/ty/issues/1648 is a bug in the user code snippet.

Ironically the one case which is not fixed by this PR is the original snippet reported by @AlexWaygood in https://github.com/astral-sh/ty/issues/1576:

```py
from typing import reveal_type, Iterable

x: Iterable[list[int]] = [[42], [56]]
reveal_type(x)
```

Even with this PR, we still reveal `list[Unknown | list[Unknown | int]]` there, instead of `Iterable[list[int]]` or `list[list[int]]`.

Overall this PR is great and we should get it merged. So if that's easy to fix here, great. If not, let's leave https://github.com/astral-sh/ty/issues/1576 open (or open a new issue) to track that case.

---

_@carljm reviewed on 2025-12-20 02:45_

(I know I said we should get this merged, but I'm out of time right now to complete the code review; will try to get back to it Monday.)

---

_Comment by @ibraheemdev on 2025-12-20 03:47_

@carljm that example should be fixed now, thanks.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-12-20 03:47_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-20 03:47_

---

_Comment by @AlexWaygood on 2025-12-20 09:33_

Looks like we're timing out on `meson` in the ecosystem CI jobs (might be worth rebasing on main to see if that magically fixes it??)

---

_@ibraheemdev reviewed on 2025-12-21 00:00_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:446 on 2025-12-21 00:00_

We could also put the `reveal_type` on the RHS expression directly.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-12-21 00:01_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-21 00:01_

---

_Comment by @ibraheemdev on 2025-12-21 01:05_

I simplified the implementation a bit, and that seems to have resolved the timeout and performance regressions.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1103 on 2025-12-30 02:19_

```suggestion
    /// If this type is a class instance, returns its class literal and specialization.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1104 on 2025-12-30 02:19_

?

```suggestion
    pub(crate) fn class_and_specialization(
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1121 on 2025-12-30 02:21_

The name of this method vs `specialization_of` no longer makes sense, now that they differ not only in optionality of `expected_class`, but also in the return type. Maybe:
```suggestion
    fn class_and_specialization_of_optional(
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8132 on 2025-12-30 02:25_

Why is it correct to just return the raw value type without applying the `UniqueSpecialization` to it at all?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:8027 on 2025-12-30 02:44_

Where is this fallback actually happening? I don't see it in the code following here.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:2075 on 2025-12-30 02:54_

Given that `forward_type_mappings` is a hashmap, would it be better to iterate `synthetic_specializations` and lookup in `forward_type_mappings`, rather than doing repeated `find`?

---

_@carljm approved on 2025-12-30 02:54_

I wish @dcreager were around to take a look at this, but he can always do a post-land review. This looks good to me, sorry for delayed review.

---

_@ibraheemdev reviewed on 2025-12-30 21:28_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:8027 on 2025-12-30 21:28_

We end up not creating an constraint for any element type variable, and so will instead infer `Unknown` [a few lines down](https://github.com/astral-sh/ruff/blob/01cd956bc8a150ea92a7d2dc8b9807d6cdd46c97/crates/ty_python_semantic/src/types/infer/builder.rs#L7959).

---

_@ibraheemdev reviewed on 2025-12-30 21:38_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:8132 on 2025-12-30 21:38_

We apply the type mapping just below after specializing the type alias, doing it here would cause the type mapping to be applied twice, and the unique specialization would be performed on a synthetic type variable the second time, instead of the actual type alias specialization.

---

_@carljm reviewed on 2025-12-30 21:44_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:8027 on 2025-12-30 21:44_

I see -- I think I was also missing that `.class_specialization(self.db()).is_some()` a couple lines below will implicitly exclude unions.

---

_@carljm reviewed on 2025-12-30 21:46_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8134 on 2025-12-30 21:46_

Does that mean we are applying all other type mappings twice?

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:8134 on 2025-12-30 21:48_

I believe so, but removing this (or even just adding `TypeMapping::Specialization(_)` to the branch above) seems to cause stack overflows and incorrect `Divergent` types, so I think this is necessary for at least some type mappings.

---

_@ibraheemdev reviewed on 2025-12-30 21:48_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:8134 on 2025-12-30 21:50_

This was added in https://github.com/astral-sh/ruff/pull/20969.

---

_@ibraheemdev reviewed on 2025-12-30 21:50_

---

_Comment by @ibraheemdev on 2025-12-30 22:03_

FWIW I discussed the core ideas behind this PR with @dcreager, so this should be good to land!

---

_Merged by @ibraheemdev on 2025-12-30 22:03_

---

_Closed by @ibraheemdev on 2025-12-30 22:03_

---

_Branch deleted on 2025-12-30 22:03_

---
