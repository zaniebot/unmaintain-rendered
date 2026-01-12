```yaml
number: 21819
title: "[ty] Narrow types after NoReturn calls in if branches"
type: pull_request
state: closed
author: alex
labels:
  - ty
assignees: []
draft: true
base: main
head: fix-noreturn-narrowing
created_at: 2025-12-06T01:25:44Z
updated_at: 2025-12-23T14:14:15Z
url: https://github.com/astral-sh/ruff/pull/21819
synced_at: 2026-01-12T15:57:34Z
```

# [ty] Narrow types after NoReturn calls in if branches

---

_@alex_

When a branch calls a NoReturn function, use the negation of the condition to narrow types after the if statement. For example, after `if val is None: sys.exit()`, `val` is now correctly narrowed to `int` instead of `int | None`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

References https://github.com/astral-sh/ty/issues/685(I don't consider this to complete it, since this PR doesn't handle `elif`-chains yet)

---

_Review requested from @carljm by @alex on 2025-12-06 01:25_

---

_Review requested from @AlexWaygood by @alex on 2025-12-06 01:25_

---

_Review requested from @sharkdp by @alex on 2025-12-06 01:25_

---

_Review requested from @dcreager by @alex on 2025-12-06 01:25_

---

_Comment by @astral-sh-bot[bot] on 2025-12-06 01:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-06 01:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/__main__.py:363:27: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_util/hint/pep/utilpepsign.py:237:12: error[invalid-return-type] Return type does not match returned value: expected `HintSign`, found `HintSign | None`
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 493 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/asyncio/connection.py:1219:17: error[unresolved-reference] Name `exceptions` used when not defined
- src/websockets/asyncio/connection.py:1235:17: error[unresolved-reference] Name `exceptions` used when not defined
- src/websockets/asyncio/connection.py:1242:29: error[unresolved-reference] Name `exceptions` used when not defined
- src/websockets/asyncio/connection.py:1243:15: error[unresolved-reference] Name `ExceptionGroup` used when not defined
- src/websockets/asyncio/connection.py:1243:51: error[unresolved-reference] Name `exceptions` used when not defined
- src/websockets/legacy/protocol.py:1610:17: error[unresolved-reference] Name `exceptions` used when not defined
- src/websockets/legacy/protocol.py:1623:17: error[unresolved-reference] Name `exceptions` used when not defined
- src/websockets/legacy/protocol.py:1630:29: error[unresolved-reference] Name `exceptions` used when not defined
- src/websockets/legacy/protocol.py:1631:15: error[unresolved-reference] Name `ExceptionGroup` used when not defined
- src/websockets/legacy/protocol.py:1631:51: error[unresolved-reference] Name `exceptions` used when not defined
- Found 41 diagnostics
+ Found 31 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/parser.py:429:9: error[invalid-assignment] Object of type `Expr` is not assignable to attribute `call` of type `Call`
- Found 181 diagnostics
+ Found 180 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/package_base.py:252:33: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'>) | (Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'>)` has no attribute `namespace`
+ lib/spack/spack/package_base.py:252:33: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'>) | (Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'>)` has no attribute `namespace`
- lib/spack/spack/package_base.py:252:55: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'>) | (Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'>)` has no attribute `name`
+ lib/spack/spack/package_base.py:252:55: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'>) | (Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'>)` has no attribute `name`
- lib/spack/spack/package_base.py:265:48: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'> & ~<Protocol with members 'determine_version'>) | (Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'> & ~<Protocol with members 'determine_version'>)` has no attribute `name`
+ lib/spack/spack/package_base.py:265:48: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'> & ~<Protocol with members 'determine_version'>) | (Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'> & ~<Protocol with members 'determine_version'>)` has no attribute `name`
- lib/spack/spack/package_base.py:265:58: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'> & ~<Protocol with members 'determine_version'>) | (Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'> & ~<Protocol with members 'determine_version'>)` has no attribute `namespace`
+ lib/spack/spack/package_base.py:265:58: error[unresolved-attribute] Object of type `(Self@__init__ & <Protocol with members 'libraries'> & ~<Protocol with members 'executables'> & ~<Protocol with members 'determine_version'>) | (Self@__init__ & <Protocol with members 'executables'> & ~<Protocol with members 'libraries'> & ~<Protocol with members 'determine_version'>)` has no attribute `namespace`

paasta (https://github.com/yelp/paasta)
- paasta_tools/cleanup_expired_autoscaling_overrides.py:72:12: warning[possibly-missing-attribute] Attribute `data` may be missing on object of type `Unknown | None`
- paasta_tools/cleanup_expired_autoscaling_overrides.py:83:44: warning[possibly-missing-attribute] Attribute `data` may be missing on object of type `Unknown | None`
- paasta_tools/cleanup_expired_autoscaling_overrides.py:136:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `data` on type `Unknown | None`
- paasta_tools/cli/cmds/mesh_status.py:111:23: warning[possibly-missing-attribute] Attribute `service` may be missing on object of type `PaastaOApiClient | None`
- paasta_tools/cli/cmds/mesh_status.py:115:12: warning[possibly-missing-attribute] Attribute `api_error` may be missing on object of type `PaastaOApiClient | None`
- paasta_tools/cli/cmds/mesh_status.py:122:13: warning[possibly-missing-attribute] Attribute `connection_error` may be missing on object of type `PaastaOApiClient | None`
- paasta_tools/cli/cmds/mesh_status.py:122:38: warning[possibly-missing-attribute] Attribute `timeout_error` may be missing on object of type `PaastaOApiClient | None`
- paasta_tools/cli/cmds/start_stop_restart.py:313:17: warning[possibly-missing-attribute] Attribute `service` may be missing on object of type `PaastaOApiClient | None`
- paasta_tools/cli/cmds/start_stop_restart.py:318:20: warning[possibly-missing-attribute] Attribute `api_error` may be missing on object of type `PaastaOApiClient | None`
- paasta_tools/cli/cmds/status.py:322:18: warning[possibly-missing-attribute] Attribute `service` may be missing on object of type `PaastaOApiClient | None`
- paasta_tools/cli/cmds/status.py:329:12: warning[possibly-missing-attribute] Attribute `api_error` may be missing on object of type `PaastaOApiClient | None`
- paasta_tools/cli/cmds/status.py:332:13: warning[possibly-missing-attribute] Attribute `connection_error` may be missing on object of type `PaastaOApiClient | None`
- paasta_tools/cli/cmds/status.py:332:38: warning[possibly-missing-attribute] Attribute `timeout_error` may be missing on object of type `PaastaOApiClient | None`
- Found 1101 diagnostics
+ Found 1088 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/cmdline.py:186:11: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, ScrapyCommand].__getitem__(key: str, /) -> ScrapyCommand` cannot be called with key of type `str | None` on object of type `dict[str, ScrapyCommand]`
- scrapy/pipelines/files.py:463:58: error[invalid-argument-type] Argument to bound method `_get_store` is incorrect: Expected `str`, found `(str & ~AlwaysFalsy) | (PathLike[str] & ~AlwaysFalsy)`
+ scrapy/pipelines/files.py:463:58: error[invalid-argument-type] Argument to bound method `_get_store` is incorrect: Expected `str`, found `(str & ~AlwaysFalsy) | (PathLike[str] & ~AlwaysFalsy & ~AlwaysTruthy)`
- Found 1735 diagnostics
+ Found 1734 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/raises.py:1427:56: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type`, found `type[BaseException] | None`
- Found 446 diagnostics
+ Found 445 diagnostics

mkosi (https://github.com/systemd/mkosi)
- mkosi/__init__.py:2748:12: error[invalid-return-type] Return type does not match returned value: expected `Path`, found `Path | None`
- mkosi/__init__.py:4447:15: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.key"]`
- mkosi/__init__.py:4447:40: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.crt"]`
- mkosi/__init__.py:4471:24: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.key"]`
- mkosi/__init__.py:4472:21: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.crt"]`
- mkosi/__init__.py:4485:9: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.version"]`
- mkosi/config.py:291:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/config.py:497:16: error[invalid-return-type] Return type does not match returned value: expected `Architecture`, found `Unknown | Architecture | None`
- mkosi/config.py:540:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/config.py:562:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/config.py:706:12: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `bool | None`
- mkosi/distribution/arch.py:125:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distribution/azure.py:112:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distribution/centos.py:96:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distribution/debian.py:257:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distribution/fedora.py:260:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distribution/kali.py:65:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distribution/mageia.py:67:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distribution/openmandriva.py:63:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distribution/opensuse.py:252:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/distribution/opensuse.py:277:18: warning[possibly-missing-attribute] Attribute `iter` may be missing on object of type `Element[str] | None`
- mkosi/distribution/postmarketos.py:111:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | str | None`
- mkosi/qemu.py:349:33: error[invalid-assignment] Object of type `list[Path | str | None]` is not assignable to `list[Path | str]`
- mkosi/qemu.py:531:13: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[Path | str]`, found `list[Unknown | Path | None | str]`
- Found 97 diagnostics
+ Found 73 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/specs/openapi/schemas.py:327:32: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `Mapping[str, Any] | None`
- Found 272 diagnostics
+ Found 271 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- test/test_response.py:669:31: error[not-iterable] Object of type `None` is not iterable
- test/test_response.py:720:31: error[not-iterable] Object of type `None` is not iterable
- test/test_response.py:775:33: error[not-iterable] Object of type `None` is not iterable
- Found 272 diagnostics
+ Found 269 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- tests/utils.py:95:17: warning[possibly-missing-attribute] Attribute `group` may be missing on object of type `Match[str] | None`
- tests/utils.py:96:14: warning[possibly-missing-attribute] Attribute `group` may be missing on object of type `Match[str] | None`
- tests/utils.py:97:47: warning[possibly-missing-attribute] Attribute `groups` may be missing on object of type `Match[str] | None`
- Found 673 diagnostics
+ Found 670 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/annotations.py:1949:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
- tanjun/annotations.py:1996:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
- Found 132 diagnostics
+ Found 130 diagnostics

mypy (https://github.com/python/mypy)
- mypy/test/data.py:146:23: warning[possibly-missing-attribute] Attribute `group` may be missing on object of type `Match[str] | None`
- mypy/test/data.py:149:36: warning[possibly-missing-attribute] Attribute `group` may be missing on object of type `Match[str] | None`
- Found 1792 diagnostics
+ Found 1790 diagnostics

vision (https://github.com/pytorch/vision)
- release/apply-release-changes.py:89:20: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal[".github"]`
- Found 1474 diagnostics
+ Found 1473 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- pyodide-build/pyodide_build/pywasmcross.py:384:24: error[no-matching-overload] No overload of function `run` matches arguments
- pyodide-build/pyodide_build/pywasmcross.py:392:27: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 876 diagnostics
+ Found 874 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- tests/unittests/sources/test_ibmcloud.py:278:56: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:279:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:280:33: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:281:65: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:282:32: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:304:41: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:305:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:306:33: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:307:63: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:326:41: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:327:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:328:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/unittests/sources/test_ibmcloud.py:329:63: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 1179 diagnostics
+ Found 1166 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/main.py:391:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[MutableMapping[str, None | int | str | ... omitted 3 union elements] | None, str, Loader]`, found `tuple[None | int | float | ... omitted 3 union elements, Unknown | str, Loader]`
+ cwltool/main.py:391:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[MutableMapping[str, None | int | str | ... omitted 3 union elements] | None, str, Loader]`, found `tuple[None | (int & Top[MutableMapping[Unknown, Unknown]]) | (float & Top[MutableMapping[Unknown, Unknown]]) | ... omitted 3 union elements, Unknown | str, Loader]`
- cwltool/main.py:477:33: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
- cwltool/main.py:499:37: error[invalid-argument-type] Argument to bound method `_init_job` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
- cwltool/main.py:502:43: error[invalid-argument-type] Argument to bound method `bind_input` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements] | list[MutableMapping[str, None | int | str | ... omitted 3 union elements]]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
- cwltool/main.py:505:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- cwltool/main.py:510:13: error[invalid-argument-type] Argument to function `printdeps` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
- cwltool/main.py:523:13: error[invalid-argument-type] Argument to bound method `store` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
- cwltool/main.py:526:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["cwl:tool"]` and `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
- cwltool/main.py:527:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- cwltool/main.py:528:8: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["__id"]` and `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
- cwltool/main.py:529:13: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- cwltool/main.py:530:12: error[invalid-return-type] Return type does not match returned value: expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `MutableMapping[str, None | int | str | ... omitted 3 union elements] | None | dict[Unknown | str, Unknown] | dict[Unknown, Unknown]`
- Found 238 diagnostics
+ Found 227 diagnostics

meson (https://github.com/mesonbuild/meson)
- packaging/createmsi.py:131:31: error[invalid-argument-type] Argument to function `check_call` is incorrect: Expected `Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes]`, found `list[Unknown | str | None]`
- Found 1918 diagnostics
+ Found 1917 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/cli/block.py:462:59: warning[possibly-missing-attribute] Attribute `fields` may be missing on object of type `BlockSchema | None`
- src/prefect/cli/block.py:464:43: warning[possibly-missing-attribute] Attribute `fields` may be missing on object of type `BlockSchema | None`
- src/prefect/cli/deployment.py:1043:12: warning[possibly-missing-attribute] Attribute `is_completed` may be missing on object of type `State[Any] | None`
- src/prefect/cli/deployment.py:1045:54: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `State[Any] | None`
- src/prefect/cli/deployment.py:1048:43: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `State[Any] | None`
- src/prefect/cli/events.py:177:9: error[invalid-argument-type] Argument is incorrect: Expected `Resource`, found `dict[Unknown, Unknown] | Any`
+ src/prefect/cli/events.py:177:9: error[invalid-argument-type] Argument is incorrect: Expected `Resource`, found `dict[Unknown, Unknown] | (Any & Top[dict[Unknown, Unknown]])`
- src/prefect/cli/flow_run.py:88:41: error[invalid-argument-type] Argument to function `run_sync_in_worker_thread` is incorrect: Expected `str`, found `str | None`
- src/prefect/cli/flow_run.py:224:17: warning[possibly-missing-attribute] Attribute `state_details` may be missing on object of type `State[Any] | None`
- src/prefect/cli/flow_run.py:225:20: warning[possibly-missing-attribute] Attribute `is_scheduled` may be missing on object of type `State[Any] | None`
- src/prefect/cli/flow_run.py:226:22: warning[possibly-missing-attribute] Attribute `timestamp` may be missing on object of type `State[Any] | None`
- src/prefect/cli/flow_run.py:232:21: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `State[Any] | None`
- src/prefect/cli/flow_run.py:428:35: error[invalid-argument-type] Argument to bound method `execute_flow_run` is incorrect: Expected `UUID`, found `UUID | None`
- src/prefect/cli/task_run.py:82:41: error[invalid-argument-type] Argument to function `run_sync_in_worker_thread` is incorrect: Expected `str`, found `str | None`
- src/prefect/cli/variable.py:90:29: warning[possibly-missing-attribute] Attribute `model_dump` may be missing on object of type `Variable | None`
- src/prefect/cli/work_queue.py:47:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `UUID | str`
+ src/prefect/cli/work_queue.py:47:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `(UUID & ~AlwaysFalsy) | (str & ~AlwaysFalsy)`
- src/prefect/cli/work_queue.py:54:21: error[invalid-argument-type] Argument to bound method `read_work_queue_by_name` is incorrect: Expected `str`, found `UUID | str`
+ src/prefect/cli/work_queue.py:54:21: error[invalid-argument-type] Argument to bound method `read_work_queue_by_name` is incorrect: Expected `str`, found `(UUID & ~AlwaysFalsy) | (str & ~AlwaysFalsy)`
- src/prefect/cli/worker.py:158:14: error[call-non-callable] Object of type `None` is not callable
- Found 5480 diagnostics
+ Found 5466 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/tests/test_core_metadata.py:407:24: error[call-non-callable] Object of type `None` is not callable
- Found 1271 diagnostics
+ Found 1270 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/commands/ddtrace_run.py:143:43: warning[possibly-missing-attribute] Attribute `command` may be missing on object of type `None | Unknown`
+ ddtrace/commands/ddtrace_run.py:143:43: warning[possibly-missing-attribute] Attribute `command` may be missing on object of type `None | (Unknown & ~None)`

AutoSplit (https://github.com/Toufool/AutoSplit)
- src/AutoSplit.py:1027:9: warning[possibly-missing-attribute] Attribute `ignore` may be missing on object of type `QCloseEvent | None`
- Found 70 diagnostics
+ Found 69 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataset.py:4586:37: error[invalid-argument-type] Argument to function `either_dict_or_kwargs` is incorrect: Expected `Mapping[Any, Any] | None`, found `Hashable`
- Found 1756 diagnostics
+ Found 1755 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/web_urldispatcher.py:920:22: error[call-non-callable] Object of type `None` is not callable
- Found 178 diagnostics
+ Found 177 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- mypy_django_plugin/config.py:72:25: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 436 diagnostics
+ Found 435 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/serialization.py:660:40: error[no-matching-overload] No overload of function `__new__` matches arguments
- src/bokeh/events.py:202:17: error[call-non-callable] Object of type `None` is not callable
- Found 860 diagnostics
+ Found 858 diagnostics

CPython (Argument Clinic) (https://github.com/python/cpython)
- Tools/clinic/libclinic/app.py:173:16: error[invalid-return-type] Return type does not match returned value: expected `Destination`, found `Destination | None`
- Tools/clinic/libclinic/dsl_parser.py:376:13: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 21 diagnostics
+ Found 19 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/auxfiletools.py:751:25: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `type[Parameter] | None`
- hydpy/core/auxfiletools.py:755:21: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `type[Parameter] | None`
- hydpy/core/devicetools.py:2401:9: error[type-assertion-failure] Type `Literal["inlets", "outlets", "observers", "receivers", "senders", "inputs", "outputs"]` is not equivalent to `Never`
- hydpy/models/sw1d_network.py:1173:74: error[invalid-argument-type] Argument to bound method `append_submodel` is incorrect: Expected `RoutingModel_V2 | RoutingModel_V3`, found `RoutingModel_V1 | RoutingModel_V2 | RoutingModel_V3`
- Found 698 diagnostics
+ Found 694 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/tests/window/test_ewm.py:709:13: error[call-non-callable] Object of type `None` is not callable
- pandas/tests/window/test_ewm.py:711:18: error[call-non-callable] Object of type `None` is not callable
- Found 3762 diagnostics
+ Found 3760 diagnostics

scipy (https://github.com/scipy/scipy)
- tools/authors.py:165:24: warning[possibly-missing-attribute] Attribute `group` may be missing on object of type `Match[str] | None`
- tools/authors.py:166:24: warning[possibly-missing-attribute] Attribute `group` may be missing on object of type `Match[str] | None`
- Found 8058 diagnostics
+ Found 8056 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/egraph.py:1959:24: error[invalid-argument-type] Argument to function `expr_action` is incorrect: Expected `BaseExpr`, found `(BaseExpr & ~Action) | (Fact & ~Action)`
- python/egglog/pretty.py:378:9: error[type-assertion-failure] Type `RulesetDecl | CombinedRulesetDecl | RewriteDecl | ... omitted 27 union elements` is not equivalent to `Never`
+ python/egglog/pretty.py:378:9: error[type-assertion-failure] Type `(RulesetDecl & ~LitDecl) | (CombinedRulesetDecl & ~LitDecl) | (RewriteDecl & ~LitDecl) | ... omitted 26 union elements` is not equivalent to `Never`
- Found 1492 diagnostics
+ Found 1491 diagnostics

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
- pydantic/v1/utils.py:613:16: error[invalid-return-type] Return type does not match returned value: expected `Mapping[int | str, Any]`, found `AbstractSet[int | str] | Mapping[int | str, Any] | dict[Unknown, ellipsis]`
+ pydantic/v1/utils.py:613:16: error[invalid-return-type] Return type does not match returned value: expected `Mapping[int | str, Any]`, found `(AbstractSet[int | str] & Top[Mapping[Unknown, object]]) | Mapping[int | str, Any] | dict[Unknown, ellipsis]`


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~35MB
+     memo metadata = ~36MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~167MB
+     memo metadata = ~176MB


```

</details>




---

_Comment by @alex on 2025-12-06 01:40_

Looking at those results, most of them look as expected. The werkzeug ones have some errors that look like may indicate a bug (there's a bunch of "attribute may not exist on Foo | None" => "attribute doesn't exist on None"). Is there a good workflow for reproducing those locally /  minimizing?

---

_Comment by @alex on 2025-12-06 01:50_

Or I can just stare at code the fold fashioned way, minimal reproducer:

```py 
from typing_extensions import reveal_type


def foo(arg: int) -> int | None:
    pass

def bar() -> None:
    f = foo(1)
    assert f is None

    f = foo(2)
    reveal_type(f)
```

---

_Comment by @codspeed-hq[bot] on 2025-12-06 01:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21819 will **degrade performances by 10.26%**

<sub>Comparing <code>alex:fix-noreturn-narrowing</code> (6c73a5c) with <code>main</code> (6e0e49e)</sub>



### Summary

`❌ 7` regressions  
`✅ 15` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 55.7 s | 60.9 s | -8.55% |
| ❌ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 198.5 s | 214.4 s | -7.44% |
| ❌ | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.8 s | 2.9 s | -4.18% |
| ❌ | WallTime | [`` small[altair] ``](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 5.5 s | 6 s | -8.7% |
| ❌ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 20.2 s | 21.2 s | -4.44% |
| ❌ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 69.2 s | 77.1 s | -10.26% |
| ❌ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.2 s | 1.3 s | -6.28% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%3Afix-noreturn-narrowing?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @alex on 2025-12-06 03:06_

Regression seems plausible given this adds some additional logic in the core of the type inference algorithm.

---

_Label `ty` added by @AlexWaygood on 2025-12-06 10:03_

---

_Comment by @TomerBin on 2025-12-06 21:23_

Wouldn't https://github.com/astral-sh/ty/issues/690 be necessary for if-elif chains and nested if cases? 

---

_Converted to draft by @alex on 2025-12-07 02:41_

---

_Comment by @alex on 2025-12-07 02:41_

(Marking as draft as I've found another issue)


---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:980 on 2025-12-09 01:03_

I'm pretty pessimistic that any approach relying on node-index ordering here is going to work out well, since source order does not always correspond to control flow. It can work for simple `if` statements...

I suspect that the best way to solve this is to do what I mentioned at the end of the description of https://github.com/astral-sh/ty/issues/685, which is to treat every new narrowing of a name as a new definition of that name (which refers to the previous live definition(s)) -- then we'll get the correct control-flow handling. 

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:1135 on 2025-12-09 01:10_

This effectively redoes a chunk of work that we already did in determining reachability of the definition in the first place, since we have to re-walk the reachability TDD. Adding extra work like this for every inference of every definition is likely to slow things down a lot, as you observed, since this is a very hot path.

It's likely that any fix for this and https://github.com/astral-sh/ty/issues/685 will carry some cost, though.

---

_@carljm reviewed on 2025-12-09 01:11_

Thanks for working on this!

---

_@alex reviewed on 2025-12-09 13:16_

---

_Review comment by @alex on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:980 on 2025-12-09 13:16_

I'm off the charts not happy with this code :-) Once I realized _something_ was necessary to change how narrowing intersected with re-binding, this was one of the dumber things that could work. But it's why I left this in draft, because I'm not happy with this as landable.

Which is all by way of saying: I appreciate the pointer to the past comment, will take a look!

---

_@carljm reviewed on 2025-12-09 18:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:980 on 2025-12-09 18:56_

Fair warning, that solution is going to be invasive; it's not a small project.

---

_@alex reviewed on 2025-12-10 01:31_

---

_Review comment by @alex on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:980 on 2025-12-10 01:31_

:-) I'm not sure where this fits in your own prioritization, but I'm likely to look at this over the holidays.

---

_@carljm reviewed on 2025-12-10 02:14_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:980 on 2025-12-10 02:14_

This is pretty high priority for us; I would have otherwise hoped to work on it in the next month or so. Not opposed to you giving it a try if you want, happy to offer some guidance as needed/useful.

---

_Comment by @alex on 2025-12-10 02:22_

👍, if you beat me to it I shall not take offense.

All that is necessary for evil to succeed is for good people to do nothing.

On Tue, Dec 9, 2025, 9:15 PM Carl Meyer ***@***.***> wrote:

> ***@***.**** commented on this pull request.
> ------------------------------
>
> In
> crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs
> <https://github.com/astral-sh/ruff/pull/21819#discussion_r2604947756>:
>
> > +        // Get the binding's NodeIndex. We'll use this to filter out predicates that were
> +        // created before this binding (and thus are about an earlier binding of the same place).
> +        let binding_index = binding.kind(db).target_node_index();
>
> This is pretty high priority for us; I would have otherwise hoped to work
> on it in the next month or so. Not opposed to you giving it a try if you
> want, happy to offer some guidance as needed/useful.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/pull/21819#discussion_r2604947756>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAAAGBEBRMKJOIV7T4MV75D4A563PAVCNFSM6AAAAACOGVSNYWVHI2DSMVQWIX3LMV43YUDVNRWFEZLROVSXG5CSMV3GSZLXHMZTKNRQGQZTINZXGU>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-18 16:32_

---

_Comment by @alex on 2025-12-23 14:14_

Spent several hours on this in the last few days without a lot to show for it, so I'm going to go ahead and close this.

---

_Closed by @alex on 2025-12-23 14:14_

---

_Branch deleted on 2025-12-23 14:14_

---
