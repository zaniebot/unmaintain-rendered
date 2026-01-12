```yaml
number: 21747
title: "[ty] Simple subtyping support for bidirectional inference"
type: pull_request
state: closed
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: ibraheem/bidi-subtyping
created_at: 2025-12-02T05:32:35Z
updated_at: 2025-12-18T02:11:27Z
url: https://github.com/astral-sh/ruff/pull/21747
synced_at: 2026-01-12T15:57:32Z
```

# [ty] Simple subtyping support for bidirectional inference

---

_@ibraheemdev_

## Summary

This solves most of https://github.com/astral-sh/ty/issues/1576. More advanced cases likely require the new constraint solver.

---

_Review requested from @carljm by @ibraheemdev on 2025-12-02 05:32_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-02 05:32_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-02 05:32_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-02 05:32_

---

_Label `ty` added by @ibraheemdev on 2025-12-02 05:32_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 05:35_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-11 15:23:42.591717927 +0000
+++ new-output.txt	2025-12-11 15:23:46.361715453 +0000
@@ -808,7 +808,7 @@
 overloads_evaluation.py:291:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@example6`
 protocols_class_objects.py:58:16: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA1`
 protocols_class_objects.py:59:16: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA2`
-protocols_definition.py:30:11: error[invalid-argument-type] Argument to function `close_all` is incorrect: Expected `Iterable[SupportsClose]`, found `list[Unknown | int]`
+protocols_definition.py:30:11: error[invalid-argument-type] Argument to function `close_all` is incorrect: Expected `Iterable[SupportsClose]`, found `list[SupportsClose | int]`
 protocols_definition.py:114:22: error[invalid-assignment] Object of type `Concrete2_Bad1` is not assignable to `Template2`
 protocols_definition.py:115:22: error[invalid-assignment] Object of type `Concrete2_Bad2` is not assignable to `Template2`
 protocols_definition.py:116:22: error[invalid-assignment] Object of type `Concrete2_Bad3` is not assignable to `Template2`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-02 05:37_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paroxython (https://github.com/laowantong/paroxython)
+ paroxython/derived_labels_db.py:189:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[LabelName, dict[Span, Any]].__getitem__(key: LabelName, /) -> dict[Span, Any]` cannot be called with key of type `str | Unknown` on object of type `defaultdict[LabelName, dict[Span, Any]]`
- Found 7 diagnostics
+ Found 8 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/__init__.py:301:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `nullcontext[None]`, found `nullcontext[object]`
- Found 172 diagnostics
+ Found 173 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/jinja2/environment.py:1114:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[Unknown, Unknown]`, found `Mapping[str, Any] | dict[Unknown, Unknown]`
+ lib/spack/spack/vendor/jinja2/environment.py:1114:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, Any]`, found `Mapping[str, Any]`

pip (https://github.com/pypa/pip)
+ src/pip/_internal/commands/search.py:101:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `dict[Unknown | str, Unknown | str | list[Unknown | str]]` on object of type `OrderedDict[str, TransformedHit]`
- src/pip/_vendor/resolvelib/structs.py:203:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `() -> Iterable[Unknown]`, found `(Iterable[CT@build_iter_view] & (() -> object)) | (() -> Iterable[CT@build_iter_view])`
+ src/pip/_vendor/resolvelib/structs.py:203:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `() -> Iterable[CT@build_iter_view]`, found `(Iterable[CT@build_iter_view] & (() -> object)) | (() -> Iterable[CT@build_iter_view])`
+ src/pip/_vendor/resolvelib/structs.py:206:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[CT@build_iter_view]`, found `(Iterable[CT@build_iter_view] & Sequence[object] & ~(() -> object)) | list[CT@build_iter_view]`
- Found 603 diagnostics
+ Found 605 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/claw/_ast/_kind/clawastimport.py:622:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/claw/_ast/_kind/clawastimport.py:631:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`

werkzeug (https://github.com/pallets/werkzeug)
- tests/test_routing.py:615:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Rule]`, found `list[Unknown | Submount | EndpointPrefix | Subdomain]`
+ tests/test_routing.py:615:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Rule]`, found `list[Rule | Submount | EndpointPrefix | Subdomain]`
- tests/test_send_file.py:131:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
+ tests/test_send_file.py:131:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[tuple[str, str] | str, Unknown | str]`
- tests/test_test.py:223:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
+ tests/test_test.py:223:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[tuple[str, str] | str, Unknown | str]`
- tests/test_test.py:227:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
+ tests/test_test.py:227:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[tuple[str, str] | str, Unknown | str]`

jinja (https://github.com/pallets/jinja)
- tests/test_loader.py:283:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[BaseLoader]`, found `list[Unknown | BaseLoader | None]`
+ tests/test_loader.py:283:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[BaseLoader]`, found `list[BaseLoader | Unknown | None]`
- tests/test_loader.py:293:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, BaseLoader]`, found `dict[Unknown | str, Unknown | BaseLoader | None]`
+ tests/test_loader.py:293:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, BaseLoader]`, found `dict[str, BaseLoader | Unknown | None]`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:1676:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
+ src/graphql/execution/execute.py:1676:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult | CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
- src/graphql/execution/execute.py:1828:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[CoroutineType[Any, Any, StreamItemResult] | StreamItemResult]`
+ src/graphql/execution/execute.py:1828:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[StreamItemResult | CoroutineType[Any, Any, StreamItemResult]]`
- src/graphql/type/definition.py:805:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/type/definition.py:909:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 427 diagnostics
+ Found 425 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/instance/kubernetes.py:381:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | str | int | list[Unknown]`
- paasta_tools/kubernetes_tools.py:3177:17: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `Unknown | list[Unknown] | str`
- paasta_tools/setup_kubernetes_cr.py:307:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:307:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:311:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:311:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:314:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:314:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:317:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:317:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:318:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["annotations"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:318:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:319:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:319:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:320:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:320:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/setup_kubernetes_cr.py:321:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["labels"]` on object of type `str`
- paasta_tools/setup_kubernetes_cr.py:321:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- paasta_tools/utils.py:609:17: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
+ paasta_tools/utils.py:609:17: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[DockerParameter | dict[Unknown | str, Unknown | str]]`
- paasta_tools/utils.py:617:35: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
+ paasta_tools/utils.py:617:35: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[DockerParameter | dict[Unknown | str, Unknown | str]]`
- paasta_tools/utils.py:630:16: error[invalid-return-type] Return type does not match returned value: expected `Iterable[DockerParameter]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
+ paasta_tools/utils.py:630:16: error[invalid-return-type] Return type does not match returned value: expected `Iterable[DockerParameter]`, found `list[DockerParameter | dict[Unknown | str, Unknown | str]]`
- Found 1105 diagnostics
+ Found 1087 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_contracts.py:571:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[Unknown | CustomFailContractPreProcess]`
+ tests/test_contracts.py:571:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[type[Contract] | CustomFailContractPreProcess]`
- tests/test_contracts.py:585:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[Unknown | CustomFailContractPostProcess]`
+ tests/test_contracts.py:585:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[type[Contract] | CustomFailContractPostProcess]`
- tests/test_http_response.py:282:35: error[invalid-argument-type] Argument to bound method `follow_all` is incorrect: Expected `Iterable[str | Link]`, found `list[Unknown | None]`
+ tests/test_http_response.py:282:35: error[invalid-argument-type] Argument to bound method `follow_all` is incorrect: Expected `Iterable[str | Link]`, found `list[str | Link | None] & list[Unknown | None]`
- tests/test_http_response.py:291:35: error[invalid-argument-type] Argument to bound method `follow_all` is incorrect: Expected `Iterable[str | Link]`, found `list[Unknown | None]`
+ tests/test_http_response.py:291:35: error[invalid-argument-type] Argument to bound method `follow_all` is incorrect: Expected `Iterable[str | Link]`, found `list[str | Link | None] & list[Unknown | None]`

ignite (https://github.com/pytorch/ignite)
- tests/ignite/metrics/nlp/test_bleu.py:26:29: error[invalid-argument-type] Argument to bound method `_corpus_bleu` is incorrect: Expected `Sequence[Sequence[Sequence[Any]]]`, found `list[Unknown | list[Unknown | int]]`
+ tests/ignite/metrics/nlp/test_bleu.py:26:29: error[invalid-argument-type] Argument to bound method `_corpus_bleu` is incorrect: Expected `Sequence[Sequence[Sequence[Any]]]`, found `list[Sequence[Sequence[Any]] | list[Sequence[Any] | int]]`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/contrib/swift.py:158:35: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[Unknown | tuple[Unknown | bytes, None, None, bool]]`
+ dulwich/contrib/swift.py:158:35: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[tuple[ObjectID, bytes | None, int | None, bool] | tuple[Unknown | bytes, None, None, bool]]`
- dulwich/contrib/swift.py:891:13: error[invalid-argument-type] Argument to bound method `add_objects` is incorrect: Expected `Sequence[tuple[ShaFile, str | None]]`, found `list[Unknown | tuple[object, None]]`
+ dulwich/contrib/swift.py:891:13: error[invalid-argument-type] Argument to bound method `add_objects` is incorrect: Expected `Sequence[tuple[ShaFile, str | None]]`, found `list[tuple[ShaFile, str | None] | tuple[object, None]]`
- dulwich/diff_tree.py:421:26: error[invalid-argument-type] Argument to function `_all_eq` is incorrect: Expected `(Unknown, /) -> Literal["delete"]`, found `def change_type(c: TreeChange) -> str`
+ dulwich/diff_tree.py:421:26: error[invalid-argument-type] Argument to function `_all_eq` is incorrect: Expected `(TreeChange | Unknown, /) -> Literal["delete"]`, found `def change_type(c: TreeChange) -> str`
- dulwich/object_store.py:2641:63: error[invalid-argument-type] Argument to function `_split_commits_and_tags` is incorrect: Expected `Iterable[ObjectID]`, found `list[Unknown | bytes]`
+ dulwich/object_store.py:2641:63: error[invalid-argument-type] Argument to function `_split_commits_and_tags` is incorrect: Expected `Iterable[ObjectID]`, found `list[bytes | Unknown]`
- dulwich/object_store.py:2812:31: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[Unknown | tuple[Unknown | bytes, None, Unknown | int, bool]]`
+ dulwich/object_store.py:2812:31: error[invalid-argument-type] Argument to bound method `add_todo` is incorrect: Expected `Iterable[tuple[ObjectID, bytes | None, int | None, bool]]`, found `list[tuple[ObjectID, bytes | None, int | None, bool] | tuple[Unknown | bytes, None, Unknown | int, bool]]`
- dulwich/walk.py:332:23: error[invalid-assignment] Object of type `list[Unknown | ObjectID | (Sequence[ObjectID] & bytes)]` is not assignable to `ObjectID | Sequence[ObjectID]`
+ dulwich/walk.py:332:23: error[invalid-assignment] Object of type `list[ObjectID | (Sequence[ObjectID] & bytes)]` is not assignable to `ObjectID | Sequence[ObjectID]`

PyGithub (https://github.com/PyGithub/PyGithub)
- github/RepositoryAdvisory.py:153:13: error[invalid-argument-type] Argument to bound method `add_vulnerabilities` is incorrect: Expected `Iterable[SimpleAdvisoryVulnerability | AdvisoryVulnerability]`, found `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str | None] | str | None | list[str]]]`
- github/RepositoryAdvisory.py:199:28: error[invalid-argument-type] Argument to bound method `offer_credits` is incorrect: Expected `Iterable[SimpleCredit | AdvisoryCredit]`, found `list[Unknown | dict[Unknown | str, Unknown | str | NamedUser]]`
- tests/RepositoryAdvisory.py:141:13: error[invalid-argument-type] Argument to bound method `offer_credits` is incorrect: Expected `Iterable[SimpleCredit | AdvisoryCredit]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
- Found 354 diagnostics
+ Found 351 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`

mkosi (https://github.com/systemd/mkosi)
- mkosi/__init__.py:2563:17: error[invalid-argument-type] Argument to bound method `sandbox` is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | str | None | Path]`
+ mkosi/__init__.py:2563:17: error[invalid-argument-type] Argument to bound method `sandbox` is incorrect: Expected `Sequence[Path | str]`, found `list[Path | str | Unknown | None]`
- mkosi/__init__.py:4391:9: error[invalid-argument-type] Argument to function `run` is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | Path | None | str]`
+ mkosi/__init__.py:4391:9: error[invalid-argument-type] Argument to function `run` is incorrect: Expected `Sequence[Path | str]`, found `list[Path | str | None]`
+ mkosi/__init__.py:4415:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `nullcontext[None]`, found `nullcontext[Popen[str] | None]`
+ mkosi/__init__.py:4418:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `nullcontext[None]`, found `nullcontext[Popen[str] | None]`
+ mkosi/__init__.py:4424:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `nullcontext[None]`, found `nullcontext[Popen[str] | None]`
+ mkosi/__init__.py:4430:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `nullcontext[None]`, found `nullcontext[Popen[str] | None]`
- mkosi/qemu.py:531:13: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | Path | None | str]`
+ mkosi/qemu.py:531:13: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | Path | None | str] & list[Path | str | None]`
- Found 97 diagnostics
+ Found 101 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/test_http.py:588:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[Unknown | list[Unknown | bytes]]`
+ test/mitmproxy/test_http.py:588:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[tuple[bytes, bytes] | list[Unknown | bytes]]`
- test/mitmproxy/test_http.py:821:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[Unknown | tuple[bytes, str]]`
+ test/mitmproxy/test_http.py:821:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[tuple[bytes, bytes] | tuple[bytes, str]]`

urllib3 (https://github.com/urllib3/urllib3)
+ test/with_dummyserver/test_https.py:760:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `nullcontext[None]`, found `nullcontext[object]`
- Found 276 diagnostics
+ Found 277 diagnostics

vision (https://github.com/pytorch/vision)
- test/test_transforms_v2.py:3752:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int] | None`, found `list[Unknown | int | float]`
+ test/test_transforms_v2.py:3752:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int] | None`, found `list[int | float] & list[Unknown | int | float]`
- test/test_transforms_v2.py:3758:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int] | None`, found `list[Unknown | float]`
+ test/test_transforms_v2.py:3758:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int] | None`, found `list[int | float] & list[Unknown | float]`
- test/test_transforms_v2.py:4773:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int]`, found `list[Unknown | int | float]`
+ test/test_transforms_v2.py:4773:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int]`, found `list[int | float] & list[Unknown | int | float]`
- test/test_transforms_v2.py:4779:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int]`, found `list[Unknown | float]`
+ test/test_transforms_v2.py:4779:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int]`, found `list[int | float] & list[Unknown | float]`
- torchvision/ops/poolers.py:283:34: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `tuple[int] | list[int] | int`
+ torchvision/ops/poolers.py:283:34: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[Unknown]`, found `tuple[int] | list[int] | int`
- torchvision/prototype/utils/_internal.py:34:82: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[str]`, found `(Collection[str] & Sequence[object]) | list[Unknown]`
+ torchvision/prototype/utils/_internal.py:34:82: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[str]`, found `Collection[str] & Sequence[object]`

pandera (https://github.com/pandera-dev/pandera)
+ pandera/backends/polars/base.py:143:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Unknown | None` on object of type `defaultdict[str, int]`
- Found 1614 diagnostics
+ Found 1615 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/study/study.py:496:13: error[invalid-argument-type] Argument to function `_optimize` is incorrect: Expected `tuple[type[Exception], ...]`, found `tuple[object, ...]`
+ optuna/study/study.py:496:25: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[type[Exception]]`, found `Iterable[type[Exception]] | (type[Exception] & Iterable[object])`

mypy (https://github.com/python/mypy)
- mypyc/irbuild/util.py:35:44: error[invalid-assignment] Object of type `frozenset[str | Unknown]` is not assignable to `frozenset[Literal["native_class", "allow_interpreted_subclasses", "serializable", "free_list_len"]]`
- Found 1792 diagnostics
+ Found 1791 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/dictionary.py:60:20: error[invalid-return-type] Return type does not match returned value: expected `Valid[Just[A@KeyNotRequired] | Nothing] | Invalid`, found `Valid[Just[Unknown | A@KeyNotRequired]]`
- koda_validate/dictionary.py:67:20: error[invalid-return-type] Return type does not match returned value: expected `Valid[Just[A@KeyNotRequired] | Nothing] | Invalid`, found `Valid[Just[Unknown | A@KeyNotRequired]]`
- Found 400 diagnostics
+ Found 398 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/context/menu.py:57:96: error[invalid-assignment] Object of type `frozenset[CommandType | Unknown]` is not assignable to `frozenset[Literal[CommandType.USER, CommandType.MESSAGE]]`
- Found 132 diagnostics
+ Found 131 diagnostics

pyppeteer (https://github.com/pyppeteer/pyppeteer)
+ pyppeteer/multimap.py:67:21: error[no-matching-overload] No overload of function `iter` matches arguments
- Found 87 diagnostics
+ Found 88 diagnostics

build (https://github.com/pypa/build)
- tests/test_main.py:270:58: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:285:58: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:302:58: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:330:62: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:340:62: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:346:68: error[invalid-argument-type] Argument to function `build_package` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:357:78: error[invalid-argument-type] Argument to function `build_package_via_sdist` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:368:92: error[invalid-argument-type] Argument to function `build_package_via_sdist` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- tests/test_main.py:373:82: error[invalid-argument-type] Argument to function `build_package_via_sdist` is incorrect: Expected `Sequence[Literal["sdist", "wheel", "editable"]]`, found `list[Unknown | str]`
- Found 56 diagnostics
+ Found 47 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: @Todo, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
+ pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[str, ((...) -> Coroutine[Any, Any, None]) | ((credentials: @Todo, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None])]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
- pymongo/helpers_shared.py:160:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: Connection) -> None) | ((credentials: @Todo, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> None]`
+ pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[str, ((...) -> None) | ((credentials: @Todo, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None)]` is not assignable to `Mapping[str, (...) -> None]`
- Found 444 diagnostics
+ Found 443 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/plugins/fortysixelks.py:161:23: error[invalid-assignment] Object of type `list[Unknown | (str & ~Literal[""]) | None]` is not assignable to `Iterable[str] | None`
+ apprise/plugins/fortysixelks.py:161:23: error[invalid-assignment] Object of type `list[str | None]` is not assignable to `Iterable[str] | None`

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/sources/DataSourceNoCloud.py:192:17: error[invalid-argument-type] Argument to function `mergemanydict` is incorrect: Expected `Sequence[Mapping[Unknown, Unknown]]`, found `list[Unknown | dict[Unknown, Unknown] | str | None]`
+ cloudinit/sources/DataSourceNoCloud.py:192:17: error[invalid-argument-type] Argument to function `mergemanydict` is incorrect: Expected `Sequence[Mapping[Unknown, Unknown]]`, found `list[Mapping[Unknown, Unknown] | Unknown | str | None]`
- cloudinit/sources/DataSourceNoCloud.py:201:13: error[invalid-argument-type] Argument to function `mergemanydict` is incorrect: Expected `Sequence[Mapping[Unknown, Unknown]]`, found `list[Unknown | dict[Unknown, Unknown] | str | None | dict[Unknown | str, Unknown | str]]`
+ cloudinit/sources/DataSourceNoCloud.py:201:13: error[invalid-argument-type] Argument to function `mergemanydict` is incorrect: Expected `Sequence[Mapping[Unknown, Unknown]]`, found `list[Mapping[Unknown, Unknown] | Unknown | str | None]`
- cloudinit/sources/helpers/aliyun.py:174:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- cloudinit/sources/helpers/aliyun.py:202:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
- cloudinit/sources/helpers/aliyun.py:205:12: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | dict[Unknown, Unknown]`
- cloudinit/sources/helpers/aliyun.py:206:25: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Unknown | int | dict[Unknown, Unknown]`
- cloudinit/sources/helpers/aliyun.py:207:13: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- cloudinit/sources/helpers/aliyun.py:208:13: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- cloudinit/sources/helpers/aliyun.py:209:13: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- cloudinit/sources/helpers/aliyun.py:210:13: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `str` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
+ tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `LiteralString` on object of type `list[Unknown | dict[Unknown | str, Unknown | str | int]]`
- tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `str` on object of type `list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1034:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["etc/cloud/ds-identify.cfg"]` and value of type `LiteralString` on object of type `list[Unknown | str]`
- Found 1178 diagnostics
+ Found 1170 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/backend/ninjabackend.py:1692:37: error[unresolved-attribute] Object of type `File` has no attribute `name`
+ mesonbuild/backend/ninjabackend.py:1695:17: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown` and value of type `CustomTarget | CustomTargetIndex | GeneratedList` on object of type `OrderedDict[str, File]`
- mesonbuild/build.py:1295:9: error[invalid-assignment] Object of type `list[Sequence[str | bool] | Literal[False]]` is not assignable to attribute `install_dir` of type `list[str | Literal[False]]`
+ mesonbuild/build.py:1295:9: error[invalid-assignment] Object of type `list[str | Literal[False] | list[str | Literal[False]]]` is not assignable to attribute `install_dir` of type `list[str | Literal[False]]`
+ mesonbuild/mconf.py:290:17: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]].__getitem__(key: str, /) -> dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]` cannot be called with key of type `Unknown | str | None` on object of type `defaultdict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]]`
+ mesonbuild/modules/codegen.py:91:45: error[no-matching-overload] No overload of function `field` matches arguments
- mesonbuild/modules/codegen.py:317:26: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Literal["lex", "flex", "reflex", "win_flex"]]`, found `list[Unknown | str]`
- mesonbuild/modules/codegen.py:398:55: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | list[str]]` is not assignable to `Mapping[Literal["yacc", "byacc", "bison", "win_bison"], list[str]]`
- mesonbuild/modules/i18n.py:425:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[str | BuildTarget | CustomTarget | ... omitted 4 union elements]`, found `list[Unknown | ExternalProgram | Executable | None | str]`
+ mesonbuild/modules/i18n.py:425:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[str | BuildTarget | CustomTarget | ... omitted 4 union elements]`, found `list[str | BuildTarget | CustomTarget | ... omitted 5 union elements]`
- mesonbuild/modules/rust.py:478:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[str | File | BuildTarget | ... omitted 5 union elements]`, found `list[Unknown | File | CustomTarget | ... omitted 5 union elements]`
+ mesonbuild/modules/rust.py:478:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[str | File | BuildTarget | ... omitted 5 union elements]`, found `list[str | File | BuildTarget | ... omitted 7 union elements]`
- mesonbuild/mparser.py:681:47: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `Mapping[str, Literal["==", "!=", "<", "<=", ">=", ... omitted 3 literals]]`
- mesonbuild/mparser.py:692:47: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `Mapping[str, Literal["+", "-", "*", "/", "%"]]`
- mesonbuild/mparser.py:697:47: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `Mapping[str, Literal["+", "-", "*", "/", "%"]]`
- Found 1953 diagnostics
+ Found 1952 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/interactions.py:229:58: error[invalid-argument-type] Argument to bound method `_from_value` is incorrect: Expected `Sequence[Literal[0, 1, 2]]`, found `list[Unknown | int]`
- Found 552 diagnostics
+ Found 551 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/__init__.py:88:46: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `<class 'IInterfaceDeclaration'>`
+ src/zope/interface/__init__.py:88:46: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[Unknown]`, found `<class 'IInterfaceDeclaration'>`

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/federation/schema.py:80:17: error[invalid-assignment] Object of type `list[Unknown | NewType]` is not assignable to `Iterable[type]`
+ strawberry/federation/schema.py:80:17: error[invalid-assignment] Object of type `list[type | NewType]` is not assignable to `Iterable[type]`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/dist.py:74:12: error[invalid-return-type] Return type does not match returned value: expected `str | list[str]`, found `Iterable[str] | list[Unknown]`
+ setuptools/_distutils/dist.py:74:12: error[invalid-return-type] Return type does not match returned value: expected `str | list[str]`, found `Iterable[str]`
- setuptools/_vendor/inflect/__init__.py:3001:13: error[no-matching-overload] No overload of bound method `join` matches arguments
+ setuptools/glob.py:34:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[AnyStr@glob]`, found `Iterator[str]`
+ setuptools/glob.py:34:23: error[invalid-argument-type] Argument to function `iglob` is incorrect: Expected `str`, found `AnyStr@glob`
- Found 1289 diagnostics
+ Found 1290 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_implementations.py:2293:51: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str | Builds[Any]`, found `(~None & ~Top[partial[Unknown]]) | Any`
- Found 546 diagnostics
+ Found 547 diagnostics

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

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/_states.py:219:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[State[Any] | R@return_value_to_state_sync]`
+ src/prefect/_states.py:219:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[R@return_value_to_state_sync | State[Any]]`
- src/prefect/server/models/concurrency_limits_v2.py:16:12: error[invalid-return-type] Return type does not match returned value: expected `ColumnElement[int | float]`, found `greatest[int]`
- src/prefect/server/models/concurrency_limits_v2.py:69:12: error[invalid-return-type] Return type does not match returned value: expected `ColumnElement[int | float]`, found `greatest[int]`
- src/prefect/states.py:369:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[State[Any] | R@return_value_to_state]`
+ src/prefect/states.py:369:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[R@return_value_to_state | State[Any]]`
- Found 5599 diagnostics
+ Found 5597 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- tests/test_settings.py:700:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, str | list[str] | bool]`, found `dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]]]`
+ tests/test_settings.py:700:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, str | list[str] | bool]`, found `dict[str, str | list[str] | bool | list[str | dict[Unknown | str, Unknown | str]]]`
- Found 41 diagnostics
+ Found 42 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- setup.py:1271:5: error[invalid-argument-type] Argument to function `setup` is incorrect: Expected `Mapping[str, type[Command]]`, found `dict[Unknown | str, Unknown | <class 'CustomBuildExt'> | <class 'LibraryDownloader'> | ... omitted 3 union elements]`
+ setup.py:1271:5: error[invalid-argument-type] Argument to function `setup` is incorrect: Expected `Mapping[str, type[Command]]`, found `dict[str, type[Command] | <class 'CleanLibraries'>]`
+ tests/debugging/exploration/_coverage.py:67:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[Path, set[Unknown]].__getitem__(key: Path, /) -> set[Unknown]` cannot be called with key of type `Path | None` on object of type `defaultdict[Path, set[Unknown]]`
- Found 8471 diagnostics
+ Found 8472 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
- tests/sparse/test_construct.pyi:253:1: error[type-assertion-failure] Type `bsr_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:253:1: error[type-assertion-failure] Type `bsr_array[complexfloating[_32Bit, _32Bit]]` does not match asserted type `bsr_array[bool[bool]]`
- tests/sparse/test_construct.pyi:254:1: error[type-assertion-failure] Type `bsr_array[signedinteger[_64Bit]]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:254:1: error[type-assertion-failure] Type `bsr_array[signedinteger[_64Bit]]` does not match asserted type `bsr_array[bool[bool]]`
- tests/sparse/test_construct.pyi:255:1: error[type-assertion-failure] Type `bsr_array[float64]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:255:1: error[type-assertion-failure] Type `bsr_array[float64]` does not match asserted type `bsr_array[bool[bool]]`
- tests/sparse/test_construct.pyi:256:1: error[type-assertion-failure] Type `bsr_array[complex128]` does not match asserted type `Unknown`
+ tests/sparse/test_construct.pyi:256:1: error[type-assertion-failure] Type `bsr_array[complex128]` does not match asserted type `bsr_array[bool[bool]]`
- tests/sparse/test_construct.pyi:257:1: error[type-a

... (truncated 910 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~4MB
+     struct fields = ~3MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~54MB
+     struct fields = ~57MB


```

</details>




---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-02 05:37_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 05:46_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 41 | 71 | 164 |
| `type-assertion-failure` | 53 | 17 | 131 |
| `invalid-assignment` | 5 | 27 | 58 |
| `unused-ignore-comment` | 20 | 16 | 0 |
| `invalid-return-type` | 1 | 4 | 11 |
| `possibly-missing-attribute` | 2 | 11 | 2 |
| `no-matching-overload` | 11 | 3 | 0 |
| `unresolved-attribute` | 14 | 0 | 0 |
| `non-subscriptable` | 8 | 4 | 0 |
| `not-iterable` | 0 | 2 | 0 |
| `unsupported-operator` | 0 | 0 | 2 |
| **Total** | **155** | **155** | **368** |

**[Full report with detailed diff](https://ibraheem-bidi-subtyping.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-bidi-subtyping.ecosystem-663.pages.dev/timing))




---

_@ibraheemdev reviewed on 2025-12-02 05:53_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:58 on 2025-12-02 05:53_

This looks wrong and seems to be causing a lot of ecosystem failures, not sure exactly what's going on here. We seem to be getting a type context of `Iterable[object]` at some point.

---

_Comment by @codspeed-hq[bot] on 2025-12-02 05:57_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-subtyping?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21747 will **degrade performances by 7.08%**

<sub>Comparing <code>ibraheem/bidi-subtyping</code> (531ca7e) with <code>main</code> (5a9d6a9)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-subtyping?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-subtyping?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 21.8 s | 23.4 s | -7.08% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-subtyping?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@ibraheemdev reviewed on 2025-12-02 05:59_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:1070 on 2025-12-02 05:59_

The benchmarks seems to suggest that this is in fact problematic, but it should be simple to memoize.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1069 on 2025-12-02 19:59_

you could possibly also mention protocols here, since although this PR tackles simple cases where a nominal type actually subclasses a protocol, there are also lots of cases where a nominal type is a subtype of a protocol without actually subclassing that protocol

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-02 22:02_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-02 22:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:40 on 2025-12-02 22:36_

any idea what's going on here?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1093 on 2025-12-02 22:38_

we do a similar loop through the MRO in the (old) constraint solver which doesn't seem to need to do the `FxHashSet` memoization thing -- could you also take a similar approach here? Might that be more performant?

https://github.com/astral-sh/ruff/blob/2250fa6f98f4fba4aaf17ff0a5e4f0be24419986/crates/ty_python_semantic/src/types/generics.rs#L1598-L1641

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6121 on 2025-12-02 22:39_

oops, sorry... I guess I baked this bug in a long time ago for you to find now :-)

---

_@AlexWaygood approved on 2025-12-02 22:39_

nice, this looks great

---

_Comment by @AlexWaygood on 2025-12-02 22:40_

(this will also help with https://github.com/astral-sh/ruff/pull/21499, where I'm currently trying (not very successfully) to do some awkward workarounds due to this missing feature 😆)

---

_@ibraheemdev reviewed on 2025-12-10 23:33_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:40 on 2025-12-10 23:33_

Fixed.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-11 02:35_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-11 02:35_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-12-11 15:51_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-11 15:51_

---

_@AlexWaygood approved on 2025-12-11 18:37_

---

_Comment by @ibraheemdev on 2025-12-18 02:11_

Closing in favor of https://github.com/astral-sh/ruff/pull/21930.

---

_Closed by @ibraheemdev on 2025-12-18 02:11_

---
