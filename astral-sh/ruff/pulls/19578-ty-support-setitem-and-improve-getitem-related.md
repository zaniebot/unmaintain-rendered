```yaml
number: 19578
title: "[ty] Support `__setitem__` and improve `__getitem__` related diagnostics"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: support-setitem
created_at: 2025-07-27T20:00:58Z
updated_at: 2025-11-06T11:49:04Z
url: https://github.com/astral-sh/ruff/pull/19578
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Support `__setitem__` and improve `__getitem__` related diagnostics

---

_Pull request opened by @MatthewMckee4 on 2025-07-27 20:00_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds validation to subscript assignment expressions.

```py
class Foo: ...

class Bar:
    __setattr__ = None

class Baz:
    def __setitem__(self, index: str, value: int) -> None:
        pass

# We now emit a diagnostic on these statements
Foo()[1] = 2
Bar()[1] = 2
Baz()[1] = 2

```

Also improves error messages on invalid `__getitem__` expressions

## Test Plan

Update mdtests and add more to `subscript/instance.md`


---

_Marked ready for review by @MatthewMckee4 on 2025-07-27 20:02_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-07-27 20:02_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-07-27 20:02_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-07-27 20:02_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-07-27 20:02_

---

_@MatthewMckee4 reviewed on 2025-07-27 20:03_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/subscript/instance.md`:54 on 2025-07-27 20:03_

This error message that was previously not tested is not great here, I think it should be updated in a following PR.

---

_Comment by @github-actions[bot] on 2025-07-27 20:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- parso/python/tokenize.py:107:16: error[call-non-callable] Method `__getitem__` of type `bound method dict[PythonVersionInfo, TokenCollection].__getitem__(key: PythonVersionInfo, /) -> TokenCollection` is not callable on object of type `dict[PythonVersionInfo, TokenCollection]`
+ parso/python/tokenize.py:107:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[PythonVersionInfo, TokenCollection].__getitem__(key: PythonVersionInfo, /) -> TokenCollection` cannot be called with key of type `tuple[Unknown, ...]` on object of type `dict[PythonVersionInfo, TokenCollection]`
- parso/python/tokenize.py:109:9: error[call-non-callable] Method `__getitem__` of type `bound method dict[PythonVersionInfo, TokenCollection].__getitem__(key: PythonVersionInfo, /) -> TokenCollection` is not callable on object of type `dict[PythonVersionInfo, TokenCollection]`
+ parso/python/tokenize.py:109:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[PythonVersionInfo, TokenCollection].__setitem__(key: PythonVersionInfo, value: TokenCollection, /) -> None` cannot be called with a key of type `tuple[Unknown, ...]` and a value of type `Unknown` on object of type `dict[PythonVersionInfo, TokenCollection]`

beartype (https://github.com/beartype/beartype)
- beartype/_check/error/_pep/pep484585/errpep484585container.py:62:9: error[call-non-callable] Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` is not callable on object of type `dict[HintSign, range]`
- beartype/_check/error/_pep/pep484585/errpep484585mapping.py:56:9: error[call-non-callable] Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` is not callable on object of type `dict[HintSign, range]`
+ beartype/_check/error/_pep/pep484585/errpep484585container.py:62:9: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` cannot be called with key of type `HintSign | None` on object of type `dict[HintSign, range]`
+ beartype/_check/error/_pep/pep484585/errpep484585mapping.py:56:9: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` cannot be called with key of type `HintSign | None` on object of type `dict[HintSign, range]`

zipp (https://github.com/jaraco/zipp)
- zipp/compat/overlay.py:37:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 3 diagnostics
+ Found 2 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/contrib/models/efficient_vit/nn/act.py:43:19: error[call-non-callable] Method `__getitem__` of type `bound method dict[str, @Todo(unsupported type[X] special form)].__getitem__(key: str, /) -> @Todo(unsupported type[X] special form)` is not callable on object of type `dict[str, @Todo(unsupported type[X] special form)]`
+ kornia/contrib/models/efficient_vit/nn/act.py:43:19: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, @Todo(unsupported type[X] special form)].__getitem__(key: str, /) -> @Todo(unsupported type[X] special form)` cannot be called with key of type `str | None` on object of type `dict[str, @Todo(unsupported type[X] special form)]`
- kornia/core/mixin/image_module.py:68:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/module.py:74:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 799 diagnostics
+ Found 797 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ psycopg/psycopg/types/json.py:124:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[tuple[type, CodeType], type[Dumper]].__setitem__(key: tuple[type, CodeType], value: type[Dumper], /) -> None` cannot be called with a key of type `tuple[type, CodeType]` and a value of type `type` on object of type `dict[tuple[type, CodeType], type[Dumper]]`
+ psycopg/psycopg/types/json.py:144:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[tuple[type, CodeType], type[Loader]].__setitem__(key: tuple[type, CodeType], value: type[Loader], /) -> None` cannot be called with a key of type `tuple[type, CodeType]` and a value of type `type` on object of type `dict[tuple[type, CodeType], type[Loader]]`
- psycopg/psycopg/types/json.py:294:16: error[call-non-callable] Method `__getitem__` of type `bound method dict[tuple[type[_JsonWrapper], PyFormat], type[Dumper]].__getitem__(key: tuple[type[_JsonWrapper], PyFormat], /) -> type[Dumper]` is not callable on object of type `dict[tuple[type[_JsonWrapper], PyFormat], type[Dumper]]`
+ psycopg/psycopg/types/json.py:294:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[tuple[type[_JsonWrapper], PyFormat], type[Dumper]].__getitem__(key: tuple[type[_JsonWrapper], PyFormat], /) -> type[Dumper]` cannot be called with key of type `tuple[type, PyFormat]` on object of type `dict[tuple[type[_JsonWrapper], PyFormat], type[Dumper]]`
- tests/types/test_multirange.py:50:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/types/test_multirange.py:73:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

trio (https://github.com/python-trio/trio)
- src/trio/_highlevel_serve_listeners.py:52:25: error[call-non-callable] Method `__getitem__` of type `bound method Mapping[int, str].__getitem__(key: int, /) -> str` is not callable on object of type `Mapping[int, str]`
+ src/trio/_highlevel_serve_listeners.py:52:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Mapping[int, str].__getitem__(key: int, /) -> str` cannot be called with key of type `int | None` on object of type `Mapping[int, str]`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/worktree.py:858:28: error[call-non-callable] Method `__getitem__` of type `bound method ReftableRefsContainer.__getitem__(name: bytes) -> Unknown | bytes` is not callable on object of type `ReftableRefsContainer`
+ dulwich/worktree.py:858:28: error[invalid-argument-type] Method `__getitem__` of type `bound method ReftableRefsContainer.__getitem__(name: bytes) -> Unknown | bytes` cannot be called with key of type `str | bytes` on object of type `ReftableRefsContainer`
- dulwich/worktree.py:863:17: error[call-non-callable] Method `__getitem__` of type `bound method ReftableRefsContainer.__getitem__(name: bytes) -> Unknown | bytes` is not callable on object of type `ReftableRefsContainer`
- Found 159 diagnostics
+ Found 158 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/__init__.py:1192:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- bson/__init__.py:1194:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- bson/__init__.py:1198:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pymongo/asynchronous/collection.py:888:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pymongo/asynchronous/collection.py:969:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pymongo/synchronous/collection.py:887:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pymongo/synchronous/collection.py:968:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 410 diagnostics
+ Found 403 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- test/common.py:272:35: error[call-non-callable] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
+ test/common.py:272:35: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["translation"]` on object of type `str`
- test/integration/test_seq_copy_int.py:242:23: error[call-non-callable] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
+ test/integration/test_seq_copy_int.py:242:23: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["translation"]` on object of type `str`

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/main.py:109:5: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
- Found 764 diagnostics
+ Found 765 diagnostics

optuna (https://github.com/optuna/optuna)
- tests/test_cli.py:1218:20: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ tests/test_cli.py:1218:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown]`
- tests/test_cli.py:1219:25: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ tests/test_cli.py:1219:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params"]` on object of type `list[Unknown]`
- tests/test_cli.py:1220:20: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ tests/test_cli.py:1220:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params"]` on object of type `list[Unknown]`
- tests/test_cli.py:1274:20: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ tests/test_cli.py:1274:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown]`
- tests/test_cli.py:1275:25: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ tests/test_cli.py:1275:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params_x"]` on object of type `list[Unknown]`
- tests/test_cli.py:1276:20: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ tests/test_cli.py:1276:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params_y"]` on object of type `list[Unknown]`
- tests/test_cli.py:1314:20: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ tests/test_cli.py:1314:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown]`
- tests/test_cli.py:1315:20: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ tests/test_cli.py:1315:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params"]` on object of type `list[Unknown]`
- tests/test_cli.py:1352:20: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
+ tests/test_cli.py:1352:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown]`

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/sources/DataSourceVMware.py:995:17: error[call-non-callable] Method `__getitem__` of type `bound method dict[str, dict[str, Interface]].__getitem__(key: str, /) -> dict[str, Interface]` is not callable on object of type `dict[str, dict[str, Interface]]`
- cloudinit/sources/DataSourceVMware.py:1003:17: error[call-non-callable] Method `__getitem__` of type `bound method dict[str, dict[str, Interface]].__getitem__(key: str, /) -> dict[str, Interface]` is not callable on object of type `dict[str, dict[str, Interface]]`
+ cloudinit/sources/DataSourceVMware.py:995:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[str, Interface]].__getitem__(key: str, /) -> dict[str, Interface]` cannot be called with key of type `None | Unknown` on object of type `dict[str, dict[str, Interface]]`
+ cloudinit/sources/DataSourceVMware.py:1003:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[str, Interface]].__getitem__(key: str, /) -> dict[str, Interface]` cannot be called with key of type `None | Unknown` on object of type `dict[str, dict[str, Interface]]`

nox (https://github.com/wntrblm/nox)
+ nox/_resolver.py:166:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Node, Literal[False]].__setitem__(key: Node, value: Literal[False], /) -> None` cannot be called with a key of type `Node` and a value of type `Literal[True]` on object of type `dict[Node, Literal[False]]`
- Found 23 diagnostics
+ Found 24 diagnostics

pybind11 (https://github.com/pybind/pybind11)
+ tests/test_sequences_and_iterators.py:157:5: error[invalid-assignment] Cannot assign to object of type `reversed[Unknown]` with no `__setitem__` method
- Found 209 diagnostics
+ Found 210 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ aiohttp_devtools/runserver/serve.py:375:17: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ aiohttp_devtools/runserver/serve.py:381:25: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | under_cached_property[Unknown]` is possibly unbound
- Found 56 diagnostics
+ Found 58 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/plugins/apprise_api.py:344:13: error[call-non-callable] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
+ apprise/plugins/apprise_api.py:344:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[Unknown, Unknown] | str` is possibly unbound
+ apprise/plugins/dbus.py:427:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/dbus.py:435:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/dbus.py:438:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/dbus.py:442:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/dbus.py:445:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/email/base.py:1121:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MIMEMultipart.__setitem__(name: str, val: str) -> None) | (bound method MIMEText.__setitem__(name: str, val: str) -> None)` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
+ apprise/plugins/email/base.py:1157:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MIMEMultipart.__setitem__(name: str, val: str) -> None) | (bound method MIMEText.__setitem__(name: str, val: str) -> None)` cannot be called with a key of type `Unknown` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
+ apprise/plugins/email/base.py:1159:13: error[invalid-assignment] Method `__setitem__` of type `(bound method MIMEMultipart.__setitem__(name: str, val: str) -> None) | (bound method MIMEText.__setitem__(name: str, val: str) -> None)` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
+ apprise/plugins/glib.py:372:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/glib.py:379:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/glib.py:383:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/glib.py:388:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/glib.py:391:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/gnome.py:260:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/gnome.py:268:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/gnome.py:273:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/macosx.py:240:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/macosx.py:246:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/macosx.py:250:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/ses.py:470:13: error[invalid-assignment] Method `__setitem__` of type `(bound method MIMEMultipart.__setitem__(name: str, val: str) -> None) | (bound method MIMEText.__setitem__(name: str, val: str) -> None)` cannot be called with a key of type `Literal["Subject"]` and a value of type `Header` on object of type `MIMEMultipart | MIMEText`
+ apprise/plugins/telegram.py:1174:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/telegram.py:1178:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/telegram.py:1188:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/telegram.py:1192:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/telegram.py:1196:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/telegram.py:1199:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/telegram.py:1203:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/telegram.py:1206:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/telegram.py:1209:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/telegram.py:1214:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/windows.py:275:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ apprise/plugins/windows.py:282:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[str, Any] | None` is possibly unbound
+ tests/test_apprise_config.py:265:5: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown]` is possibly unbound
+ tests/test_apprise_config.py:576:5: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown]` is possibly unbound
+ tests/test_attach_http.py:310:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, str].__setitem__(key: str, value: str, /) -> None` cannot be called with a key of type `Literal["Content-Length"]` and a value of type `Unknown | Literal[1048576001]` on object of type `dict[str, str]`
+ tests/test_plugin_dbus.py:96:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["gi.repository"]` and a value of type `SimpleNamespace` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_dbus.py:158:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["dbus.mainloop.qt"]` and a value of type `None` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_dbus.py:159:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["dbus.mainloop.glib"]` and a value of type `None` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_dbus.py:380:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["dbus.mainloop.glib"]` and a value of type `SimpleNamespace` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_dbus.py:383:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["gi"]` and a value of type `SimpleNamespace` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_dbus.py:384:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["gi.repository"]` and a value of type `SimpleNamespace` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_dbus.py:628:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["dbus.mainloop.glib"]` and a value of type `SimpleNamespace` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_dbus.py:631:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["gi"]` and a value of type `SimpleNamespace` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_dbus.py:632:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["gi.repository"]` and a value of type `SimpleNamespace` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_glib.py:74:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["gi.repository"]` and a value of type `SimpleNamespace` on object of type `dict[str, ModuleType]`
+ tests/test_plugin_glib.py:106:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ModuleType].__setitem__(key: str, value: ModuleType, /) -> None` cannot be called with a key of type `Literal["gi.repository"]` and a value of type `SimpleNamespace` on object of type `dict[str, ModuleType]`
- Found 1708 diagnostics
+ Found 1754 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/test_misc.py:163:26: error[call-non-callable] Method `__getitem__` of type `bound method _Environ[str].__getitem__(key: str) -> str` is not callable on object of type `_Environ[str]`
+ tests/test_misc.py:163:26: error[invalid-argument-type] Method `__getitem__` of type `bound method _Environ[str].__getitem__(key: str) -> str` cannot be called with key of type `None | str` on object of type `_Environ[str]`

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/commands/buddydump.py:189:5: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: tuple[str, str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[tuple[str, str]], /) -> None]` cannot be called with a key of type `Literal[0]` and a value of type `Unknown | tuple[None, None]` on object of type `list[tuple[str, str]]`
+ pwndbg/commands/buddydump.py:192:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: tuple[str, str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[tuple[str, str]], /) -> None]` cannot be called with a key of type `Literal[1]` and a value of type `Unknown | tuple[None, None]` on object of type `list[tuple[str, str]]`
+ pwndbg/commands/buddydump.py:195:13: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: tuple[str, str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[tuple[str, str]], /) -> None]` cannot be called with a key of type `Literal[2]` and a value of type `Unknown | tuple[None, None]` on object of type `list[tuple[str, str]]`
+ pwndbg/commands/buddydump.py:235:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: tuple[str, str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[tuple[str, str]], /) -> None]` cannot be called with a key of type `Literal[1]` and a value of type `tuple[Literal["per_cpu_pageset"], None]` on object of type `list[tuple[str, str]]`
+ pwndbg/commands/buddydump.py:261:5: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: tuple[str, str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[tuple[str, str]], /) -> None]` cannot be called with a key of type `Literal[1]` and a value of type `tuple[Literal["free_area"], None]` on object of type `list[tuple[str, str]]`
+ pwndbg/commands/buddydump.py:281:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: tuple[str, str], /) -> None, (key: slice[Any, Any, Any], value: Iterable[tuple[str, str]], /) -> None]` cannot be called with a key of type `Literal[0]` and a value of type `tuple[str, None]` on object of type `list[tuple[str, str]]`
+ pwndbg/commands/comments.py:36:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, str].__setitem__(key: str, value: str, /) -> None` cannot be called with a key of type `str` and a value of type `Unknown | None` on object of type `dict[str, str]`
- pwndbg/commands/cymbol.py:119:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pwndbg/commands/misc.py:18:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pwndbg/dbg/lldb/__init__.py:1601:13: error[call-non-callable] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
+ pwndbg/dbg/lldb/__init__.py:1601:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, Any].__setitem__(key: str, value: Any, /) -> None` cannot be called with a key of type `None | str` and a value of type `def handler(_frame: Unknown, _bp_loc: Unknown, _struct: Unknown, _internal) -> bool` on object of type `dict[str, Any]`
- Found 2326 diagnostics
+ Found 2331 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ tornado/websocket.py:1022:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, bool].__setitem__(key: str, value: bool, /) -> None` cannot be called with a key of type `Literal["max_wbits"]` and a value of type `int` on object of type `dict[str, bool]`
+ tornado/websocket.py:1024:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, bool].__setitem__(key: str, value: bool, /) -> None` cannot be called with a key of type `Literal["max_wbits"]` and a value of type `int` on object of type `dict[str, bool]`
+ tornado/websocket.py:1025:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, bool].__setitem__(key: str, value: bool, /) -> None` cannot be called with a key of type `Literal["compression_options"]` and a value of type `dict[str, Any] | None` on object of type `dict[str, bool]`
- Found 243 diagnostics
+ Found 246 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/metrics/test_accuracy.py:317:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_accuracy.py:317:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_accuracy.py:317:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_accuracy.py:317:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_accuracy.py:317:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_accuracy.py:317:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_accuracy.py:375:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_accuracy.py:375:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_accuracy.py:375:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_accuracy.py:375:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_accuracy.py:375:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_accuracy.py:375:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_accuracy.py:376:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_accuracy.py:376:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_accuracy.py:376:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_accuracy.py:376:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_accuracy.py:376:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_accuracy.py:376:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_accuracy.py:444:33: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_accuracy.py:444:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_accuracy.py:444:33: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_accuracy.py:444:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_accuracy.py:444:33: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_accuracy.py:444:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_classification_report.py:28:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_classification_report.py:28:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_classification_report.py:28:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_classification_report.py:28:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_classification_report.py:28:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_classification_report.py:28:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_classification_report.py:102:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_classification_report.py:102:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_classification_report.py:102:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_classification_report.py:102:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_classification_report.py:102:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_classification_report.py:102:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_classification_report.py:103:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_classification_report.py:103:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_classification_report.py:103:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_classification_report.py:103:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_classification_report.py:103:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_classification_report.py:103:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_epoch_metric.py:171:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_epoch_metric.py:171:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_epoch_metric.py:171:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_epoch_metric.py:171:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_epoch_metric.py:171:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_epoch_metric.py:171:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_fbeta.py:135:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_fbeta.py:135:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_fbeta.py:135:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_fbeta.py:135:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_fbeta.py:135:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_fbeta.py:135:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_average_precision.py:189:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_average_precision.py:189:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_average_precision.py:189:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_mean_average_precision.py:189:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_average_precision.py:189:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_average_precision.py:189:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_average_precision.py:190:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_average_precision.py:190:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_average_precision.py:190:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_mean_average_precision.py:190:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_average_precision.py:190:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_average_precision.py:190:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_metrics_lambda.py:440:28: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_metrics_lambda.py:440:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_metrics_lambda.py:440:28: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_metrics_lambda.py:440:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_metrics_lambda.py:440:28: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_metrics_lambda.py:440:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_metrics_lambda.py:441:28: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_metrics_lambda.py:441:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_metrics_lambda.py:441:28: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_metrics_lambda.py:441:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_metrics_lambda.py:441:28: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_metrics_lambda.py:441:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_metrics_lambda.py:489:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_metrics_lambda.py:489:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_metrics_lambda.py:489:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_metrics_lambda.py:489:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_metrics_lambda.py:489:13: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_metrics_lambda.py:489:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_precision.py:351:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_precision.py:351:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[int | float]`
- tests/ignite/metrics/test_precision.py:351:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_precision.py:351:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[str]`
- tests/ignite/metrics/test_precision.py:351:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_precision.py:351:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], slice[None, None, None]]` on object of type `list[Any]`
- tests/ignite/metrics/test_precision.py:403:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_precision.py:403:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_precision.py:403:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` is not callable on object of type `list[str]`
+ tests/ignite/metrics/test_precision.py:403:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[str]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[str]`
- tests/ignite/metrics/test_precision.py:403:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
+ tests/ignite/metrics/test_precision.py:403:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[Any]`
- tests/ignite/metrics/test_precision.py:404:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` is not callable on object of type `list[int | float]`
+ tests/ignite/metrics/test_precision.py:404:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | float, (s: slice[Any, Any, Any], /) -> list[int | float]]` cannot be called with key of type `tuple[slice[Any, Any, Any], ellipsis]` on object of type `list[int | float]`
- tests/ignite/metrics/test_precision.py:404:21: error[call-non-callable] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> str, (s: slice[Any, Any, Any], /) -> list[...*[Comment body truncated]*

---

_Label `ty` added by @AlexWaygood on 2025-07-27 21:09_

---

_Renamed from "Support `__setitem__`" to "[ty] Support `__setitem__`" by @AlexWaygood on 2025-07-27 21:09_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-07-27 21:43_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-28 07:45_

---

_Comment by @github-actions[bot] on 2025-07-28 07:54_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `call-non-callable` | 0 | 182 | 0 |
| `invalid-argument-type` | 154 | 0 | 0 |
| `invalid-assignment` | 114 | 0 | 0 |
| `possibly-unbound-implicit-call` | 75 | 0 | 0 |
| `unused-ignore-comment` | 0 | 20 | 0 |
| `possibly-unresolved-reference` | 0 | 2 | 0 |
| **Total** | **343** | **204** | **0** |


---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:321 on 2025-07-29 08:01_

Maybe similar to your comment below, but this error message is really confusing, because it seems to imply that something is wrong with the type on which `__setitem__` is called. But it's just the `value` argument which doesn't match.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/subscript/instance.md`:46 on 2025-07-29 08:08_

```suggestion
## `__getitem__` with invalid index argument
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/subscript/instance.md`:58 on 2025-07-29 08:08_

```suggestion
## `__setitem__` with no `__getitem__`
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/subscript/instance.md`:69 on 2025-07-29 08:09_

```suggestion
## Subscript store with no `__setitem__`
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/subscript/instance.md`:78 on 2025-07-29 08:09_

```suggestion
## `__setitem__` not callable
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/subscript/instance.md`:88 on 2025-07-29 08:10_

I would move this section (the basic, valid use case) further up, and show the error cases later.
```suggestion
## Valid `__setitem__` method
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/subscript/instance.md`:99 on 2025-07-29 08:11_

```suggestion
## `__setitem__` with invalid index argument
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/subscript/instance.md`:107 on 2025-07-29 08:11_

Similar here. I think we should improve this error message.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:567 on 2025-07-29 08:14_

For invalid attribute assignments, we simply use the `invalid-assignment` diagnostic, instead of creating a dedicated new lint rule (there is `invalid-attribute-access`, but that's used for something slightly different, like when trying to assign to an instance attribute from a class object). So I think we should do the same here?

/cc @MichaReiser 

---

_@sharkdp reviewed on 2025-07-29 08:18_

Thank you very much, this looks good.

Did you have a look at the ecosystem changes? You can download a detailed diff with links to the original source [here](https://github.com/astral-sh/ruff/actions/runs/16563228420/artifacts/3627119532). It looks like some diagnostics simply transformed from `non-subscriptable` to `invalid-item-assignment`, but there are also a few new diagnostics which might be worth looking into.

---

_@MatthewMckee4 reviewed on 2025-07-29 13:04_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:321 on 2025-07-29 13:04_

Yeah exactly, I'll try to change that in this PR then, for `__getitem__` too

---

_Comment by @github-actions[bot] on 2025-07-29 13:08_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-01 07:08:56.976522715 +0000
+++ new-output.txt	2025-08-01 07:08:57.039522808 +0000
@@ -368,8 +368,8 @@
 generics_basic.py:34:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr` and `AnyStr`
 generics_basic.py:139:5: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_basic.py:140:5: error[type-assertion-failure] Argument does not have asserted type `int`
-generics_basic.py:157:5: error[call-non-callable] Method `__getitem__` of type `bound method MyMap1[str, int].__getitem__(key: str, /) -> int` is not callable on object of type `MyMap1[str, int]`
-generics_basic.py:158:5: error[call-non-callable] Method `__getitem__` of type `bound method MyMap2[int, str].__getitem__(key: str, /) -> int` is not callable on object of type `MyMap2[int, str]`
+generics_basic.py:157:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap1[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap1[str, int]`
+generics_basic.py:158:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap2[int, str].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap2[int, str]`
 generics_basic.py:162:12: error[invalid-argument-type] `<class 'int'>` is not a valid argument to `Generic`
 generics_basic.py:163:12: error[invalid-argument-type] `<class 'int'>` is not a valid argument to `Protocol`
 generics_basic.py:171:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
@@ -704,6 +704,7 @@
 namedtuples_usage.py:30:1: error[type-assertion-failure] Argument does not have asserted type `str`
 namedtuples_usage.py:31:1: error[type-assertion-failure] Argument does not have asserted type `int`
 namedtuples_usage.py:32:1: error[type-assertion-failure] Argument does not have asserted type `int`
+namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to object of type `Point` with no `__setitem__` method
 namedtuples_usage.py:49:1: error[type-assertion-failure] Argument does not have asserted type `int`
 namedtuples_usage.py:50:1: error[type-assertion-failure] Argument does not have asserted type `str`
 narrowing_typeguard.py:17:9: error[type-assertion-failure] Argument does not have asserted type `tuple[str, str]`
@@ -724,7 +725,7 @@
 narrowing_typeis.py:96:9: error[type-assertion-failure] Argument does not have asserted type `B`
 narrowing_typeis.py:132:20: error[invalid-argument-type] Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeIs[int]`
 narrowing_typeis.py:191:18: error[invalid-argument-type] Argument to function `takes_int_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def bool_typeis(val: object) -> TypeIs[bool]`
-overloads_basic.py:39:1: error[call-non-callable] Method `__getitem__` of type `Overload[(__i: int) -> int, (__s: slice[Any, Any, Any]) -> bytes]` is not callable on object of type `Bytes`
+overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int) -> int, (__s: slice[Any, Any, Any]) -> bytes]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
 overloads_definitions.py:20:5: error[invalid-overload] Overloaded function `func1` requires at least two overloads
 overloads_definitions.py:33:5: error[invalid-overload] Overloaded non-stub function `func2` must have an implementation
 overloads_definitions.py:63:9: error[invalid-overload] Overloaded non-stub function `not_abstract` must have an implementation
@@ -888,4 +889,4 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 889 diagnostics
+Found 890 diagnostics
```
</details>


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-29 15:25_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-29 15:25_

---

_@MatthewMckee4 reviewed on 2025-07-29 16:06_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:321 on 2025-07-29 16:06_

Updated them, they look better but not sure if they cover all cases of `BindingError`

---

_@MichaReiser reviewed on 2025-07-30 09:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:567 on 2025-07-30 09:13_

I don't have much context on the existing rules. The reasons for separate rules are:

* They have different accuracy (which may result in different default severities or categories)
* If they are conceptually different enough or people have different opinions on one aspect and the other (there are good reasons to only enable one but not the other)

Otherwise, I'd go with `invalid-assignment`

---

_@sharkdp reviewed on 2025-07-30 09:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:567 on 2025-07-30 09:43_

I would claim that none of these criteria apply here, so I would suggest we remove that new lint rule and use `invalid-assignment`.

---

_Comment by @sharkdp on 2025-07-30 09:44_

> Did you have a look at the ecosystem changes? You can download a detailed diff with links to the original source [here](https://github.com/astral-sh/ruff/actions/runs/16563228420/artifacts/3627119532). It looks like some diagnostics simply transformed from `non-subscriptable` to `invalid-item-assignment`, but there are also a few new diagnostics which might be worth looking into.

Just to clarify: I haven't reviewed again since it's still not clear to me if the ecosystem impact is correct. Let me know if you need help with that, happy to take a more detailed look.

---

_@MatthewMckee4 reviewed on 2025-07-30 09:44_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/diagnostic.rs`:567 on 2025-07-30 09:44_

Sounds good

---

_Comment by @MatthewMckee4 on 2025-07-30 16:27_

I'll review again after removing the new lint rule. It's also probably beneficial if I split this PR up.

---

_Comment by @MatthewMckee4 on 2025-07-30 18:44_

hmm

```py
class Foo:
    def __setitem__(self, index: int, value: int) -> None:
        pass

Foo()["a"] = 1
```

Should invalid assignment or invalid argument type be emitted here?

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-30 18:54_

---

_Comment by @sharkdp on 2025-07-31 10:28_

> Should invalid assignment or invalid argument type be emitted here?

I think we should emit `invalid-assignment`, ideally with a subdiagnostics *why* the assignment failed (which could contain the the invalid argument type error). We don't do this for invalid *attribute* assignments yet, but eventually want to add this. So it doesn't need to be implemented here.

---

_Renamed from "[ty] Support `__setitem__`" to "[ty] Support `__setitem__` and improve `__getitem__` related diagnostics" by @MatthewMckee4 on 2025-07-31 17:26_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-07-31 17:28_

---

_Comment by @MatthewMckee4 on 2025-07-31 17:30_

@sharkdp would you be able to redo the ecosystem-analyzer? Thanks

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-31 17:42_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-31 17:42_

---

_@sharkdp approved on 2025-08-01 07:09_

Very nice — thank you!

Added a commit with a minor rewording of two diagnostic messages, because I was slightly confused when I read the old message the first time.

The typing conformance and ecosystem changes look good to me.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-01 07:09_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-01 07:09_

---

_Merged by @sharkdp on 2025-08-01 07:23_

---

_Closed by @sharkdp on 2025-08-01 07:23_

---

_Branch deleted on 2025-11-06 11:49_

---
