```yaml
number: 20950
title: "[ty] Avoid promoting invariant generics during literal promotion"
type: pull_request
state: closed
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: ibraheem/literal-promotion-soundness
created_at: 2025-10-17T20:13:39Z
updated_at: 2025-10-31T16:42:01Z
url: https://github.com/astral-sh/ruff/pull/20950
synced_at: 2026-01-12T15:57:13Z
```

# [ty] Avoid promoting invariant generics during literal promotion

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ty/issues/1284.

---

_Review requested from @carljm by @ibraheemdev on 2025-10-17 20:13_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-17 20:13_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-17 20:13_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-17 20:13_

---

_Label `ty` added by @ibraheemdev on 2025-10-17 20:13_

---

_@ibraheemdev reviewed on 2025-10-17 20:15_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:6846 on 2025-10-17 20:15_

We could technically do better and partially promote types with mixed variance generics (e.g. `Callable`), but that doesn't seem necessary (and arguably, the only type we really care recursively promoting are tuples).

---

_Comment by @github-actions[bot] on 2025-10-17 20:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-17 20:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- src/attr/_funcs.py:487:44: error[invalid-argument-type] Argument to function `get_type_hints` is incorrect: Expected `dict[str, Any] | None`, found `Unknown | None | bool`
+ src/attr/_funcs.py:487:44: error[invalid-argument-type] Argument to function `get_type_hints` is incorrect: Expected `dict[str, Any] | None`, found `Unknown | None | Literal[True]`
- src/attr/_funcs.py:487:44: error[invalid-argument-type] Argument to function `get_type_hints` is incorrect: Expected `Mapping[str, Any] | None`, found `Unknown | None | bool`
+ src/attr/_funcs.py:487:44: error[invalid-argument-type] Argument to function `get_type_hints` is incorrect: Expected `Mapping[str, Any] | None`, found `Unknown | None | Literal[True]`
- src/attr/_funcs.py:487:44: error[invalid-argument-type] Argument to function `get_type_hints` is incorrect: Expected `bool`, found `Unknown | None | bool`
+ src/attr/_funcs.py:487:44: error[invalid-argument-type] Argument to function `get_type_hints` is incorrect: Expected `bool`, found `Unknown | None | Literal[True]`

pip (https://github.com/pypa/pip)
- src/pip/_vendor/urllib3/util/ssl_.py:182:62: error[invalid-argument-type] Argument to function `wrap_socket` is incorrect: Expected `str | None`, found `Unknown | bool`
+ src/pip/_vendor/urllib3/util/ssl_.py:182:62: error[invalid-argument-type] Argument to function `wrap_socket` is incorrect: Expected `str | None`, found `Unknown | Literal[False]`

paasta (https://github.com/yelp/paasta)
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | Literal[True]`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | Literal[True]`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `LongRunningServiceConfig`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `LongRunningServiceConfig`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | Literal[True]`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `ServiceNamespaceConfig`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `ServiceNamespaceConfig`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | Literal[True]`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `Future[Unknown]`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `Future[Unknown]`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | Literal[True]`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `bool`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `bool`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | Literal[True]`

alerta (https://github.com/alerta/alerta)
- alerta/views/permissions.py:77:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ alerta/views/permissions.py:77:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["admin", "read", "write"]]`
- tests/test_scopes.py:81:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:81:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read"]]`
- tests/test_scopes.py:82:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:82:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["write"]]`
- tests/test_scopes.py:83:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:83:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["admin"]]`
- tests/test_scopes.py:85:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:85:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read:alerts"]]`
- tests/test_scopes.py:86:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:86:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["write:alerts"]]`
- tests/test_scopes.py:87:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:87:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["admin:alerts"]]`
- tests/test_scopes.py:89:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:89:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read"]]`
- tests/test_scopes.py:90:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:90:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read:blackouts", "read"]]`
- tests/test_scopes.py:92:59: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:92:59: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read:blackouts", "write:blackouts"]]`
- tests/test_scopes.py:93:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:93:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["write:blackouts"]]`
- tests/test_scopes.py:94:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:94:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read:blackouts", "write"]]`
- tests/test_scopes.py:95:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:95:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read:blackouts", "admin"]]`
- tests/test_scopes.py:96:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:96:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read", "write:keys"]]`
- tests/test_scopes.py:97:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:97:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read", "admin:keys"]]`
- tests/test_scopes.py:99:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:99:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["write"]]`
- tests/test_scopes.py:100:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:100:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["read", "write", "admin"]]`
- tests/test_scopes.py:101:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:101:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | Literal["write"]]`

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/contrib/pyopenssl.py:397:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[Any]] | None`, found `dict[str, list[Any] | tuple[tuple[tuple[str, Unknown]]]]`
+ src/urllib3/contrib/pyopenssl.py:397:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, list[Any]] | None`, found `dict[str, list[Any] | tuple[tuple[tuple[Literal["commonName"], Unknown]]]]`

antidote (https://github.com/Finistere/antidote)
+ tests/core/test_thread_safety.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `Box[tuple[str, @Todo, @Todo]]`, found `Box[tuple[Literal["lazy"], Unknown, Unknown]]`
+ tests/core/test_thread_safety.py:125:16: error[invalid-return-type] Return type does not match returned value: expected `Box[tuple[str, @Todo, @Todo]]`, found `Box[tuple[Literal["inject"], @Todo, @Todo]]`
+ tests/core/test_thread_safety.py:201:16: error[invalid-return-type] Return type does not match returned value: expected `Box[tuple[str, @Todo]]`, found `Box[tuple[Literal["f1"], Unknown]]`
+ tests/core/test_thread_safety.py:206:16: error[invalid-return-type] Return type does not match returned value: expected `Box[tuple[str, @Todo]]`, found `Box[tuple[Literal["f2"], Unknown]]`
+ tests/core/test_thread_safety.py:211:16: error[invalid-return-type] Return type does not match returned value: expected `Box[tuple[str, @Todo]]`, found `Box[tuple[Literal["f3"], Unknown]]`
- Found 301 diagnostics
+ Found 306 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/structure/page_tests.py:670:39: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[Unknown | str, Unknown | str] | str | None | dict[Unknown, Unknown]`
- mkdocs/tests/structure/page_tests.py:693:39: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[Unknown | str, Unknown | str] | str`
+ mkdocs/tests/structure/page_tests.py:670:39: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[Unknown | str, Unknown | str] | Literal["http://github.com/mkdocs/mkdocs/edit/master/docs/testing.md", "http://github.com/mkdocs/mkdocs/edit/master/docs/sub1/non-index.md", "https://github.com/mkdocs/mkdocs/edit/master/docs/testing.md", "https://github.com/mkdocs/mkdocs/edit/master/docs/sub1/non-index.md", "http://example.com/edit/master/testing.md", ... omitted 19 literals] | None | dict[Unknown, Unknown]`
+ mkdocs/tests/structure/page_tests.py:693:39: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[Unknown | str, Unknown | str] | Literal["edit/master/testing.md", "WARNING:mkdocs.structure.pages:edit_uri: 'edit/master/testing.md' is not a valid URL, it should include the http:// (scheme)"]`

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/tests/test_declarations.py:2057:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | str | ((*interfaces) -> Unknown) | InterfaceClass`
+ src/zope/interface/tests/test_declarations.py:2057:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | str | (def moduleProvides(*interfaces) -> Unknown) | InterfaceClass`

meson (https://github.com/mesonbuild/meson)
- unittests/allplatformstests.py:223:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[list[Unknown | str], str]]` is not assignable to attribute `values` of type `dict[str, tuple[str | int, str | None]]`
+ unittests/allplatformstests.py:223:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[list[Unknown | str], Literal["description"]]]` is not assignable to attribute `values` of type `dict[str, tuple[str | int, str | None]]`
- unittests/allplatformstests.py:3861:26: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[str, (node_list) -> Unknown]` with length 2
+ unittests/allplatformstests.py:3861:26: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[str, def accept_node_list(node_list) -> Unknown]` with length 2
- unittests/allplatformstests.py:3861:26: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[str, (kwargs) -> Unknown]` with length 2
+ unittests/allplatformstests.py:3861:26: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[str, def accept_kwargs(kwargs) -> Unknown]` with length 2
- unittests/allplatformstests.py:3861:26: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[str, (json_node) -> Unknown]` with length 2
+ unittests/allplatformstests.py:3861:26: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[str, def accept_node(json_node) -> Unknown]` with length 2

vision (https://github.com/pytorch/vision)
+ test/common_extended_utils.py:251:52: error[invalid-argument-type] Argument to function `matmul_flop` is incorrect: Expected `list[Any]`, found `Unknown | tuple[()]`
+ test/common_extended_utils.py:251:52: error[invalid-argument-type] Argument to function `addmm_flop` is incorrect: Expected `list[Any]`, found `Unknown | tuple[()]`
- test/common_extended_utils.py:251:52: error[invalid-argument-type] Argument is incorrect: Expected `list[Any]`, found `Unknown | tuple[()]`
+ test/common_extended_utils.py:251:52: error[invalid-argument-type] Argument to function `bmm_flop` is incorrect: Expected `list[Any]`, found `Unknown | tuple[()]`
- test/common_extended_utils.py:251:52: error[invalid-argument-type] Argument is incorrect: Expected `list[Any]`, found `Unknown | tuple[()]`
+ test/common_extended_utils.py:251:52: error[invalid-argument-type] Argument to function `conv_flop` is incorrect: Expected `list[Any]`, found `Unknown | tuple[()]`
+ test/common_extended_utils.py:251:52: error[invalid-argument-type] Argument to function `conv_backward_flop` is incorrect: Expected `list[Any]`, found `Unknown | tuple[()]`
+ test/common_extended_utils.py:251:52: error[invalid-argument-type] Argument to function `quant_conv_flop` is incorrect: Expected `list[Any]`, found `Unknown | tuple[()]`
+ test/common_extended_utils.py:251:52: error[invalid-argument-type] Argument to function `scaled_dot_product_flash_attention_flop` is incorrect: Expected `list[Any]`, found `Unknown | tuple[()]`
- test/test_transforms_v2.py:7012:30: warning[possibly-missing-attribute] Attribute `keys` on type `dict[Unknown | str, Unknown | Image | int] | tuple[Image | Unknown, Literal[1] | Unknown]` may be missing
+ test/test_transforms_v2.py:7012:30: warning[possibly-missing-attribute] Attribute `keys` on type `dict[Unknown | str, Unknown | Image | Literal[1]] | tuple[Image | Unknown, Literal[1] | Unknown]` may be missing
- Found 1465 diagnostics
+ Found 1470 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/auth.py:365:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: @Todo, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
+ pymongo/asynchronous/auth.py:365:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | (def _authenticate_gssapi(credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | (def _authenticate_x509(credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ... omitted 5 union elements]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
- pymongo/synchronous/auth.py:360:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: Connection) -> None) | ((credentials: @Todo, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> None]`
+ pymongo/synchronous/auth.py:360:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | (def _authenticate_gssapi(credentials: @Todo, conn: Connection) -> None) | (def _authenticate_x509(credentials: @Todo, conn: Connection) -> None) | ... omitted 5 union elements]` is not assignable to `Mapping[str, (...) -> None]`

xarray (https://github.com/pydata/xarray)
- xarray/core/variable.py:2207:32: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | Literal[False] | list[Unknown | bool]`
+ xarray/core/variable.py:2207:32: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | Literal[False] | list[Unknown | Literal[False]]`
- xarray/core/variable.py:2216:46: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | Literal[False] | list[Unknown | bool]`
+ xarray/core/variable.py:2216:46: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | Literal[False] | list[Unknown | Literal[False]]`
- xarray/tests/test_units.py:2333:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown] | None`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[str, Unknown]] | dict[Unknown, Unknown]`
+ xarray/tests/test_units.py:2333:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown] | None`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[Literal["x"], Unknown]] | dict[Unknown, Unknown]`
- xarray/tests/test_units.py:2333:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Hashable, Index] | None`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[str, Unknown]] | dict[Unknown, Unknown]`
+ xarray/tests/test_units.py:2333:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Hashable, Index] | None`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[Literal["x"], Unknown]] | dict[Unknown, Unknown]`
- xarray/tests/test_units.py:2333:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[str, Unknown]] | dict[Unknown, Unknown]`
+ xarray/tests/test_units.py:2333:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[Literal["x"], Unknown]] | dict[Unknown, Unknown]`
- xarray/tests/test_units.py:2369:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown] | None`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[str, Unknown]] | dict[Unknown, Unknown]`
+ xarray/tests/test_units.py:2369:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown] | None`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[Literal["x"], Unknown]] | dict[Unknown, Unknown]`
- xarray/tests/test_units.py:2369:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Hashable, Index] | None`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[str, Unknown]] | dict[Unknown, Unknown]`
+ xarray/tests/test_units.py:2369:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Hashable, Index] | None`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[Literal["x"], Unknown]] | dict[Unknown, Unknown]`
- xarray/tests/test_units.py:2369:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[str, Unknown]] | dict[Unknown, Unknown]`
+ xarray/tests/test_units.py:2369:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | dict[Unknown | str, Unknown] | dict[Unknown | str, Unknown | tuple[Literal["x"], Unknown]] | dict[Unknown, Unknown]`

apprise (https://github.com/caronc/apprise)
- apprise/plugins/apprise_api.py:344:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `dict[Unknown | str, Unknown | str] | str` may be missing
+ apprise/plugins/apprise_api.py:344:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `dict[Unknown | str, Unknown | Literal["", "info"]] | str` may be missing
- apprise/plugins/discord.py:357:25: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown] | str` may be missing
+ apprise/plugins/discord.py:357:25: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown] | Literal[""]` may be missing
- apprise/plugins/email/base.py:505:28: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `int`, in comparing `Unknown | Literal["Email"]` with `(Unknown & ~AlwaysFalsy) | (int & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | tuple[Unknown | str]`
+ apprise/plugins/email/base.py:505:28: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `int`, in comparing `Unknown | Literal["Email"]` with `(Unknown & ~AlwaysFalsy) | (int & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | tuple[Unknown | Literal["Email"]] | tuple[Unknown | Literal["UserID"]]`
- apprise/plugins/email/base.py:514:26: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `int`, in comparing `Unknown | Literal["UserID"]` with `(Unknown & ~AlwaysFalsy) | (int & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | tuple[Unknown | str]`
+ apprise/plugins/email/base.py:514:26: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `int`, in comparing `Unknown | Literal["UserID"]` with `(Unknown & ~AlwaysFalsy) | (int & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | tuple[Unknown | Literal["Email"]] | tuple[Unknown | Literal["UserID"]]`
- apprise/plugins/fcm/__init__.py:421:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown | str, Unknown | str]` may be missing
+ apprise/plugins/fcm/__init__.py:421:21: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown | str, Unknown | Literal[""]]` may be missing
- apprise/plugins/msteams.py:373:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | None | str]` may be missing
+ apprise/plugins/msteams.py:373:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | dict[Unknown | str, Unknown | None | Literal[""]]` may be missing
- apprise/plugins/notifiarr.py:330:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["text"]` on object of type `NotifyType`
+ apprise/plugins/notifiarr.py:330:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["text"]` on object of type `Literal[NotifyType.INFO]`
- apprise/plugins/notifiarr.py:331:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | NotifyType | dict[Unknown | str, Unknown | bool | str] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int] | dict[Unknown | str, Unknown | str] | dict[Unknown | str, Unknown]]` may be missing
+ apprise/plugins/notifiarr.py:331:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | Literal[NotifyType.INFO] | dict[Unknown | str, Unknown | bool | str] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | Literal[0]] | dict[Unknown | str, Unknown | Literal[""]] | dict[Unknown | str, Unknown]]` may be missing
- apprise/plugins/office365.py:390:13: warning[possibly-missing-attribute] Attribute `update` on type `Unknown | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]] | str` may be missing
+ apprise/plugins/office365.py:390:13: warning[possibly-missing-attribute] Attribute `update` on type `Unknown | dict[Unknown | str, Unknown | Literal[""] | dict[Unknown | str, Unknown | str]] | str` may be missing
- apprise/plugins/office365.py:469:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]] | str` may be missing
+ apprise/plugins/office365.py:469:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | Literal[""] | dict[Unknown | str, Unknown | str]] | str` may be missing
- apprise/plugins/office365.py:489:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]] | str` may be missing
+ apprise/plugins/office365.py:489:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | Literal[""] | dict[Unknown | str, Unknown | str]] | str` may be missing
+ apprise/plugins/office365.py:494:17: error[index-out-of-bounds] Index 0 is out of bounds for string `Literal[""]` with length 0
- apprise/plugins/office365.py:506:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]] | str` may be missing
+ apprise/plugins/office365.py:506:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | Literal[""] | dict[Unknown | str, Unknown | str]] | str` may be missing
- apprise/plugins/office365.py:513:21: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | str | dict[Unknown | str, Unknown | str]` may be missing
+ apprise/plugins/office365.py:513:21: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | Literal[""] | dict[Unknown | str, Unknown | str]` may be missing
- apprise/plugins/office365.py:536:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]] | str` may be missing
+ apprise/plugins/office365.py:536:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown | Literal[""] | dict[Unknown | str, Unknown | str]] | str` may be missing
- apprise/plugins/office365.py:543:21: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | str | dict[Unknown | str, Unknown | str]` may be missing
+ apprise/plugins/office365.py:543:21: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | Literal[""] | dict[Unknown | str, Unknown | str]` may be missing
- apprise/plugins/pagerduty.py:384:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str` may be missing
+ apprise/plugins/pagerduty.py:384:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | Literal["info", "warning", "critical"]` may be missing
- Found 2268 diagnostics
+ Found 2269 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/netinfo.py:86:17: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | str | bool | list[Unknown]` may be missing
+ cloudinit/netinfo.py:86:17: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | Literal[""] | bool | list[Unknown]` may be missing
- cloudinit/netinfo.py:100:17: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | str | bool | list[Unknown]` may be missing
+ cloudinit/netinfo.py:100:17: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | Literal[""] | bool | list[Unknown]` may be missing

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/archive_util.py:288:25: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | PathLike[str] | Unknown`
+ setuptools/_distutils/archive_util.py:288:25: error[invalid-argument-type] Argument to function `make_tarball` is incorrect: Expected `str`, found `str | PathLike[str] | Unknown`
- setuptools/_distutils/archive_util.py:288:25: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | PathLike[str] | Unknown`
+ setuptools/_distutils/archive_util.py:288:25: error[invalid-argument-type] Argument to function `make_zipfile` is incorrect: Expected `str`, found `str | PathLike[str] | Unknown`
+ setuptools/_distutils/archive_util.py:288:46: error[invalid-argument-type] Argument to function `make_tarball` is incorrect: Expected `Literal["gzip", "bzip2", "xz"] | None`, found `Unknown | bool`
- setuptools/_distutils/archive_util.py:288:46: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `Unknown | bool`
+ setuptools/_distutils/archive_util.py:288:46: error[invalid-argument-type] Argument to function `make_tarball` is incorrect: Expected `str | None`, found `Unknown | bool`
- setuptools/_distutils/archive_util.py:288:46: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `Unknown | bool`
+ setuptools/_distutils/archive_util.py:288:46: error[invalid-argument-type] Argument to function `make_tarball` is incorrect: Expected `str | None`, found `Unknown | bool`
- setuptools/_distutils/archive_util.py:288:46: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `Unknown | bool`
- setuptools/_distutils/tests/test_build_ext.py:380:16: warning[possibly-missing-attribute] Attribute `undef_macros` on type `Unknown | tuple[str, dict[Unknown | str, Unknown | list[Unknown | str] | str | list[Unknown | tuple[str, str, str] | str]]]` may be missing
+ setuptools/_distutils/tests/test_build_ext.py:380:16: warning[possibly-missing-attribute] Attribute `undef_macros` on type `Unknown | tuple[Literal["foo.bar"], dict[Unknown | str, Unknown | list[Unknown | str] | str | list[Unknown | tuple[str, str, str] | str]]]` may be missing
- setuptools/_distutils/tests/test_build_ext.py:381:16: warning[possibly-missing-attribute] Attribute `define_macros` on type `Unknown | tuple[str, dict[Unknown | str, Unknown | list[Unknown | str] | str | list[Unknown | tuple[str, str, str] | str]]]` may be missing
+ setuptools/_distutils/tests/test_build_ext.py:381:16: warning[possibly-missing-attribute] Attribute `define_macros` on type `Unknown | tuple[Literal["foo.bar"], dict[Unknown | str, Unknown | list[Unknown | str] | str | list[Unknown | tuple[str, str, str] | str]]]` may be missing
- setuptools/tests/test_warnings.py:72:39: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | tuple[str, str] | dict[Unknown | str, Unknown | int | str] | ... omitted 3 union elements`
+ setuptools/tests/test_warnings.py:72:39: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | tuple[Literal["Hello {x}"], Literal["\n\t{target} {v:.1f}"]] | dict[Unknown | str, Unknown | int | str] | ... omitted 4 union elements`
- setuptools/tests/test_warnings.py:73:48: error[invalid-argument-type] Argument to function `cleandoc` is incorrect: Expected `str`, found `Unknown | tuple[str, str] | dict[Unknown | str, Unknown | int | str] | ... omitted 3 union elements`
+ setuptools/tests/test_warnings.py:73:48: error[invalid-argument-type] Argument to function `cleandoc` is incorrect: Expected `str`, found `Unknown | tuple[Literal["Hello {x}"], Literal["\n\t{target} {v:.1f}"]] | dict[Unknown | str, Unknown | int | str] | ... omitted 4 union elements`
- setuptools/tests/test_wheel.py:677:13: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `str | dict[Unknown | str, Unknown] | dict[str, list[Unknown | str]] | Unknown`
+ setuptools/tests/test_wheel.py:677:13: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Literal["script"] | dict[Unknown | str, Unknown] | dict[str, list[Unknown | str]] | Unknown`

manticore (https://github.com/trailofbits/manticore)
- manticore/core/parser/parser.py:223:51: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ manticore/core/parser/parser.py:223:51: error[too-many-positional-arguments] Too many positional arguments to function `default_read_register`: expected 1, got 2
- manticore/core/parser/parser.py:223:51: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ manticore/core/parser/parser.py:223:51: error[too-many-positional-arguments] Too many positional arguments to function `default_get_descriptor`: expected 1, got 2
- manticore/core/parser/parser.py:232:11: error[missing-argument] No argument provided for required parameter `size`
+ manticore/core/parser/parser.py:232:11: error[missing-argument] No argument provided for required parameter `size` of function `default_read_memory`
- manticore/core/parser/parser.py:233:22: error[missing-argument] No argument provided for required parameter `size`
+ manticore/core/parser/parser.py:233:22: error[missing-argument] No argument provided for required parameter `size` of function `default_read_memory`
- manticore/core/parser/parser.py:235:51: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ manticore/core/parser/parser.py:235:51: error[too-many-positional-arguments] Too many positional arguments to function `default_read_register`: expected 1, got 2
- manticore/core/parser/parser.py:235:51: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ manticore/core/parser/parser.py:235:51: error[too-many-positional-arguments] Too many positional arguments to function `default_get_descriptor`: expected 1, got 2
- manticore/core/parser/parser.py:257:12: error[missing-argument] No argument provided for required parameter `size`
+ manticore/core/parser/parser.py:257:12: error[missing-argument] No argument provided for required parameter `size` of function `default_read_memory`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/book_providers.py:747:9: error[invalid-argument-type] Argument to function `multisort_best` is incorrect: Expected `list[tuple[Literal["min", "max"], (@Todo, /) -> int | float]]`, found `list[Unknown | tuple[str, (rec) -> Unknown]]`
- openlibrary/plugins/worksearch/code.py:299:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, Unknown | int] | tuple[str, @Todo | str]]`
+ openlibrary/plugins/worksearch/code.py:299:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[Literal["start"], Unknown | Literal[0]] | tuple[Literal["rows"], Unknown | Literal[100]] | tuple[Literal["ol.label"], @Todo | Literal["UNLABELLED"]] | tuple[Literal["wt"], Unknown]]`
- Found 865 diagnostics
+ Found 864 diagnostics

arviz (https://github.com/arviz-devs/arviz)
+ arviz/stats/diagnostics.py:201:27: error[unknown-argument] Argument `prob` does not match any known parameter of function `_ess_bulk`
+ arviz/stats/diagnostics.py:201:27: error[unknown-argument] Argument `prob` does not match any known parameter of function `_ess_mean`
- arviz/stats/diagnostics.py:201:27: error[unknown-argument] Argument `prob` does not match any known parameter
+ arviz/stats/diagnostics.py:201:27: error[unknown-argument] Argument `prob` does not match any known parameter of function `_ess_sd`
+ arviz/stats/diagnostics.py:201:27: error[unknown-argument] Argument `prob` does not match any known parameter of function `_ess_median`
+ arviz/stats/diagnostics.py:201:27: error[unknown-argument] Argument `prob` does not match any known parameter of function `_ess_mad`
+ arviz/stats/diagnostics.py:201:27: error[unknown-argument] Argument `prob` does not match any known parameter of function `_ess_z_scale`
+ arviz/stats/diagnostics.py:201:27: error[unknown-argument] Argument `prob` does not match any known parameter of function `_ess_folded`
+ arviz/stats/diagnostics.py:201:27: error[unknown-argument] Argument `prob` does not match any known parameter of function `_ess_identity`
+ arviz/stats/diagnostics.py:204:20: error[missing-argument] No argument provided for required parameter `prob` of function `_ess_quantile`
- arviz/stats/diagnostics.py:204:20: error[missing-argument] No argument provided for required parameter `prob`
+ arviz/stats/diagnostics.py:204:20: error[missing-argument] No argument provided for required parameter `prob` of function `_ess_local`
+ arviz/stats/diagnostics.py:423:40: error[unknown-argument] Argument `prob` does not match any known parameter of function `_mcse_mean`
- arviz/stats/diagnostics.py:423:40: error[unknown-argument] Argument `prob` does not match any known parameter
+ arviz/stats/diagnostics.py:423:40: error[unknown-argument] Argument `prob` does not match any known parameter of function `_mcse_sd`
+ arviz/stats/diagnostics.py:423:40: error[unknown-argument] Argument `prob` does not match any known parameter of function `_mcse_median`
- arviz/stats/diagnostics.py:425:20: error[missing-argument] No argument provided for required parameter `prob`
+ arviz/stats/diagnostics.py:425:20: error[missing-argument] No argument provided for required parameter `prob` of function `_mcse_quantile`
- Found 731 diagnostics
+ Found 741 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `Operator`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `Operator`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterId | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterId | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterName | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterName | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterTags | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterTags | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterDeploymentId | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterDeploymentId | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterWorkQueueName | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterWorkQueueName | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterFlowVersion | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterFlowVersion | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterEndTime | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterEndTime | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterExpectedStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterExpectedStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterNextScheduledStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterNextScheduledStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterParentFlowRunId | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterParentFlowRunId | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterParentTaskRunId | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterParentTaskRunId | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterIdempotencyKey | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterIdempotencyKey | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `Operator`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `Operator`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterId | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterId | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterName | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterName | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterTags | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterTags | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterDeploymentId | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterDeploymentId | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterWorkQueueName | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterWorkQueueName | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterFlowVersion | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterFlowVersion | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterEndTime | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterEndTime | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterExpectedStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterExpectedStartTime | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterParentFlowRunId | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterParentFlowRunId | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterParentTaskRunId | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterParentTaskRunId | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterIdempotencyKey | None`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
+ src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `FlowRunFilterIdempotencyKey | None`, found `dict[str, Unknown] | dict[str, Unknown | Literal[False]]`

colour (https://github.com/colour-science/colour)
- colour/colorimetry/tests/test_tristimulus_values.py:1576:26: error[invalid-argument-type] Argument is incorrect: Expected `MultiSpectralDistributions`, found `SpectralDistribution`
+ colour/colorimetry/tests/test_tristimulus_values.py:1576:26: error[invalid-argument-type] Argument to function `msds_to_XYZ_ASTME308` is incorrect: Expected `MultiSpectralDistributions`, found `SpectralDistribution`
- colour/colorimetry/tests/test_tristimulus_values.py:1589:38: error[invalid-argument-type] Argument is incorrect: Expected `SpectralDistribution`, found `MultiSpectralDistributions`
+ colour/colorimetry/tests/test_tristimulus_values.py:1589:38: error[invalid-argument-type] Argument to function `sd_to_XYZ_ASTME308` is incorrect: Expected `SpectralDistribution`, found `MultiSpectralDistributions`
- colour/colorimetry/tests/test_tristimulus_values.py:1625:38: error[invalid-argument-type] Argument is incorrect: Expected `MultiSpectralDistributions`, found `SpectralDistribution`
+ colour/colorimetry/tests/test_tristimulus_values.py:1625:38: error[invalid-argument-type] Argument to function `msds_to_XYZ_ASTME308` is incorrect: Expected `MultiSpectralDistributions`, found `SpectralDistribution`
- colour/colorimetry/tests/test_tristimulus_values.py:1638:38: error[invalid-argument-type] Argument is incorrect: Expected `SpectralDistribution`, found `MultiSpectralDistributions`
+ colour/colorimetry/tests/test_tristimulus_values.py:1638:38: error[invalid-argument-type] Argument to function `sd_to_XYZ_ASTME308` is incorrect: Expected `SpectralDistribution`, found `MultiSpectralDistributions`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/manifold/_t_sne.py:1066:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, Unknown | int].__setitem__(key: str, value: Unknown | int, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["angle"]` and a value of type `Unknown` on object of type `Unknown | int | dict[str, Unknown | int] | list[Unknown] | float`
+ sklearn/manifold/_t_sne.py:1066:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, Unknown | Literal[0]].__setitem__(key: str, value: Unknown | Literal[0], /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["angle"]` and a value of type `Unknown` on object of type `Unknown | int | dict[str, Unknown | Literal[0]] | list[Unknown] | float`
- sklearn/manifold/_t_sne.py:1068:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, Unknown | int].__setitem__(key: str, value: Unknown | int, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["verbose"]` and a value of type `Unknown` on object of type `Unknown | int | dict[str, Unknown | int] | list[Unknown] | float`
+ sklearn/manifold/_t_sne.py:1068:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, Unknown | Literal[0]].__setitem__(key: str, value: Unknown | Literal[0], /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["verbose"]` and a value of type `Unknown` on object of type `Unknown | int | dict[str, Unknown | Literal[0]] | list[Unknown] | float`
- sklearn/manifold/_t_sne.py:1071:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, Unknown | int].__setitem__(key: str, value: Unknown | int, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["num_threads"]` and a value of type `Unknown` on object of type `Unknown | int | dict[str, Unknown | int] | list[Unknown] | float`
+ sklearn/manifold/_t_sne.py:1071:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[str, Unknown | Literal[0]].__setitem__(key: str, value: Unknown | Literal[0], /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["num_threads"]` and a value of type `Unknown` on object of type `Unknown | int | dict[str, Unknown | Literal[0]] | list[Unknown] | float`
- sklearn/tree/_export.py:724:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `str | Unknown | int` may be missing
+ sklearn/tree/_export.py:724:9: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Literal["center", "axes fraction", 100] | Unknown` may be missing
- sklearn/tree/_export.py:731:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `str | Unknown | int` may be missing
+ sklearn/tree/_export.py:731:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Literal["center", "axes fraction", 100] | Unknown` may be missing
- sklearn/tree/_export.py:733:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `str | Unknown | int` may be missing
+ sklearn/tree/_export.py:733:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Literal["center", "axes fraction", 100] | Unknown` may be missing
- sklearn/tree/_export.py:767:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `str | Unknown | int` may be missing
+ sklearn/tree/_export.py:767:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Literal["center", "axes fraction", 100] | Unknown` may be missing

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/memory_measure.py:231:23: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[tuple[Literal["Total"], Any]]`, found `Iterable[tuple[str, Any]]`
- Found 1911 diagnostics
+ Found 1912 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- benchmarks/set_http_meta/scenario.py:74:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `str | int | dict[Unknown | str, Unknown | str] | Unknown | dict[Unknown | str, Unknown | int]` may be missing
+ benchmarks/set_http_meta/scenario.py:74:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Literal["GET", "OK", 200, 0] | dict[Unknown | str, Unknown | str] | Unknown | dict[Unknown | str, Unknown | int]` may be missing
- benchmarks/set_http_meta/scenario.py:80:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `str | int | dict[Unknown | str, Unknown | str] | Unknown | dict[Unknown | str, Unknown | int]` may be missing
+ benchmarks/set_http_meta/scenario.py:80:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Literal["GET", "OK", 200, 0] | dict[Unknown | str, Unknown | str] | Unknown | dict[Unknown | str, Unknown | int]` may be missing
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_api_operation_v0`
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_faas_operation_v0`
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_messaging_operation_v0`
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `database_operation_v0`
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `messaging_operation_v0`
- ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `service_name_v0`
- ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `url_operation_v0`
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_api_operation_v1`
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_faas_operation_v1`
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_messaging_operation_v1`
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `database_operation_v1`
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `messaging_operation_v1`
- ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter
+ ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `url_operation_v1`
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_api_operation_v0`
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_faas_operation_v0`
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_messaging_operation_v0`
- ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `database_operation_v0`
- ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `messaging_operation_v0`
- ddtrace/_trace/utils_valkey.py:55:59: error[unknown-argument] Argument `cache_provider` does not match any known parameter
- ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `service_name_v0`
- ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `url_operation_v0`
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_api_operation_v1`
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_faas_operation_v1`
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_messaging_operation_v1`
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `database_operation_v1`
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `messaging_operation_v1`
- ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter
+ ddtrace/_trace/utils_valkey.py:72:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `url_operation_v1`
+ ddtrace/_trace/utils_valkey.py:88:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_api_operation_v0`
+ ddtrace/_trace/utils_valkey.py:88:49: error[unknown-argument] Argument `cache_provider` does not match any known parameter of function `cloud_faas_operation_v0`
+ ddtrace/_trace/utils_valkey.py:88:49: error[unknown-argument] Argument `cache_provider` does not mat...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-17 20:17_

---

_Comment by @github-actions[bot] on 2025-10-17 20:24_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unknown-argument` | 1,189 | 1 | 629 |
| `invalid-argument-type` | 55 | 5 | 112 |
| `parameter-already-assigned` | 24 | 0 | 30 |
| `possibly-missing-attribute` | 1 | 2 | 16 |
| `too-many-positional-arguments` | 9 | 0 | 9 |
| `invalid-assignment` | 1 | 0 | 15 |
| `missing-argument` | 6 | 4 | 6 |
| `possibly-missing-implicit-call` | 0 | 0 | 16 |
| `invalid-return-type` | 5 | 0 | 3 |
| `no-matching-overload` | 2 | 6 | 0 |
| `unsupported-operator` | 0 | 0 | 7 |
| `index-out-of-bounds` | 3 | 0 | 3 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| `call-non-callable` | 1 | 0 | 0 |
| **Total** | **1,298** | **18** | **846** |

**[Full report with detailed diff](https://ibraheem-literal-promotion-s.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-literal-promotion-s.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-10-17 20:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fliteral-promotion-soundness?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #20950 will **degrade performances by 6.7%**

<sub>Comparing <code>ibraheem/literal-promotion-soundness</code> (e73d73b) with <code>main</code> (8ca2b55)</sub>



### Summary

`❌ 2` regressions  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fliteral-promotion-soundness?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fliteral-promotion-soundness?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 10.5 s | 11.2 s | -6.33% |
| ❌ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fliteral-promotion-soundness?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 32.9 s | 35.2 s | -6.7% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fliteral-promotion-soundness?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @AlexWaygood on 2025-10-18 13:51_

While there are some new diagnostics on other projects too, the vast majority of the new diagnostics appear to be new `unknown-argument` diagnostics on ddtrace, which is pretty interesting

---

_Closed by @ibraheemdev on 2025-10-31 16:42_

---
