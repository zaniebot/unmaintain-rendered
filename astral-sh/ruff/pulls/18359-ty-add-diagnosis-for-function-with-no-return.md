```yaml
number: 18359
title: "[ty] Add diagnosis for function with no return statement but with return type annotation"
type: pull_request
state: merged
author: lipefree
labels:
  - ty
assignees: []
merged: true
base: main
head: no-return-statement
created_at: 2025-05-29T00:10:54Z
updated_at: 2025-05-29T23:17:19Z
url: https://github.com/astral-sh/ruff/pull/18359
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Add diagnosis for function with no return statement but with return type annotation

---

_@lipefree_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Partially implement https://github.com/astral-sh/ty/issues/538, 
```py
from pathlib import Path

def setup_test_project(registry_name: str, registry_url: str, project_dir: str) -> Path:
    pyproject_file = Path(project_dir) / "pyproject.toml"
    pyproject_file.write_text("...", encoding="utf-8")
```
As no return statement is defined in the function `setup_test_project` with annotated return type `Path`, we provide the following diagnosis : 

- error[invalid-return-type]: Function **always** implicitly returns `None`, which is not assignable to return type `Path`

with a subdiagnostic : 
- note: Consider changing your return annotation to `-> None`

I was not able to implement the second part `if the function only has bare return statements rather than return <variable> statements` without running into issues but I can still work on it if it is required for this PR.
 
## Test Plan

<!-- How was it tested? -->

mdtests with snapshots to capture the subdiagnostic. I have to mention that existing snapshots were modified since they now fall in this category.

---

_Review requested from @carljm by @lipefree on 2025-05-29 00:10_

---

_Review requested from @AlexWaygood by @lipefree on 2025-05-29 00:10_

---

_Review requested from @sharkdp by @lipefree on 2025-05-29 00:10_

---

_Review requested from @dcreager by @lipefree on 2025-05-29 00:10_

---

_Comment by @github-actions[bot] on 2025-05-29 00:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- error[invalid-return-type] bidict/_bidict.py:41:30: Function can implicitly return `None`, which is not assignable to return type `MutableBidict[VT, KT]`
+ error[invalid-return-type] bidict/_bidict.py:41:30: Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT, KT]`
- error[invalid-return-type] bidict/_bidict.py:44:26: Function can implicitly return `None`, which is not assignable to return type `MutableBidict[VT, KT]`
+ error[invalid-return-type] bidict/_bidict.py:44:26: Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT, KT]`
- error[invalid-return-type] bidict/_bidict.py:185:30: Function can implicitly return `None`, which is not assignable to return type `bidict[VT, KT]`
+ error[invalid-return-type] bidict/_bidict.py:185:30: Function always implicitly returns `None`, which is not assignable to return type `bidict[VT, KT]`
- error[invalid-return-type] bidict/_bidict.py:188:26: Function can implicitly return `None`, which is not assignable to return type `bidict[VT, KT]`
+ error[invalid-return-type] bidict/_bidict.py:188:26: Function always implicitly returns `None`, which is not assignable to return type `bidict[VT, KT]`
- error[invalid-return-type] bidict/_frozen.py:34:30: Function can implicitly return `None`, which is not assignable to return type `frozenbidict[VT, KT]`
+ error[invalid-return-type] bidict/_frozen.py:34:30: Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT, KT]`
- error[invalid-return-type] bidict/_frozen.py:37:26: Function can implicitly return `None`, which is not assignable to return type `frozenbidict[VT, KT]`
+ error[invalid-return-type] bidict/_frozen.py:37:26: Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT, KT]`
- error[invalid-return-type] bidict/_orderedbase.py:138:30: Function can implicitly return `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
+ error[invalid-return-type] bidict/_orderedbase.py:138:30: Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
- error[invalid-return-type] bidict/_orderedbase.py:141:26: Function can implicitly return `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
+ error[invalid-return-type] bidict/_orderedbase.py:141:26: Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
- error[invalid-return-type] bidict/_orderedbidict.py:39:30: Function can implicitly return `None`, which is not assignable to return type `OrderedBidict[VT, KT]`
+ error[invalid-return-type] bidict/_orderedbidict.py:39:30: Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT, KT]`
- error[invalid-return-type] bidict/_orderedbidict.py:42:26: Function can implicitly return `None`, which is not assignable to return type `OrderedBidict[VT, KT]`
+ error[invalid-return-type] bidict/_orderedbidict.py:42:26: Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT, KT]`

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- error[invalid-return-type] tests/conftest.py:90:64: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/conftest.py:90:64: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] tests/conftest.py:93:68: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/conftest.py:93:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] tests/conftest.py:96:43: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/conftest.py:96:43: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] tests/conftest.py:99:53: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/conftest.py:99:53: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] tests/conftest.py:102:65: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/conftest.py:102:65: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] tests/conftest.py:105:77: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/conftest.py:105:77: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] tests/conftest.py:108:87: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/conftest.py:108:87: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] tests/conftest.py:113:14: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/conftest.py:113:14: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] tests/conftest.py:196:31: Function can implicitly return `None`, which is not assignable to return type `Literal[True]`
+ error[invalid-return-type] tests/conftest.py:196:31: Function always implicitly returns `None`, which is not assignable to return type `Literal[True]`
- error[invalid-return-type] tests/conftest.py:200:30: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/conftest.py:200:30: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] tests/conftest.py:206:37: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] tests/conftest.py:206:37: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] tests/type_tests.py:31:16: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] tests/type_tests.py:31:16: Function always implicitly returns `None`, which is not assignable to return type `Never`

parso (https://github.com/davidhalter/parso)
- error[invalid-return-type] parso/tree.py:137:28: Function can implicitly return `None`, which is not assignable to return type `tuple[int, int]`
+ error[invalid-return-type] parso/tree.py:137:28: Function always implicitly returns `None`, which is not assignable to return type `tuple[int, int]`
- error[invalid-return-type] parso/tree.py:145:26: Function can implicitly return `None`, which is not assignable to return type `tuple[int, int]`
+ error[invalid-return-type] parso/tree.py:145:26: Function always implicitly returns `None`, which is not assignable to return type `tuple[int, int]`

attrs (https://github.com/python-attrs/attrs)
- error[invalid-return-type] tests/test_converters.py:101:36: Function can implicitly return `None`, which is not assignable to return type `int | float`
+ error[invalid-return-type] tests/test_converters.py:101:36: Function always implicitly returns `None`, which is not assignable to return type `int | float`

kornia (https://github.com/kornia/kornia)
- error[invalid-return-type] kornia/utils/_compat.py:50:82: Function can implicitly return `None`, which is not assignable to return type `tuple[Unknown, ...]`
+ error[invalid-return-type] kornia/utils/_compat.py:50:82: Function always implicitly returns `None`, which is not assignable to return type `tuple[Unknown, ...]`

ignite (https://github.com/pytorch/ignite)
- error[invalid-return-type] ignite/handlers/tqdm_logger.py:228:10: Function can implicitly return `None`, which is not assignable to return type `RemovableEventHandle`
+ error[invalid-return-type] ignite/handlers/tqdm_logger.py:228:10: Function always implicitly returns `None`, which is not assignable to return type `RemovableEventHandle`

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-return-type] pydantic/_internal/_utils.py:307:70: Function can implicitly return `None`, which is not assignable to return type `T`
+ error[invalid-return-type] pydantic/_internal/_utils.py:307:70: Function always implicitly returns `None`, which is not assignable to return type `T`
- error[invalid-return-type] pydantic/v1/dataclasses.py:88:62: Function can implicitly return `None`, which is not assignable to return type `DataclassT`
+ error[invalid-return-type] pydantic/v1/dataclasses.py:88:62: Function always implicitly returns `None`, which is not assignable to return type `DataclassT`

nox (https://github.com/wntrblm/nox)
- error[invalid-return-type] nox/_cli.py:141:6: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] nox/_cli.py:141:6: Function always implicitly returns `None`, which is not assignable to return type `Never`

trio (https://github.com/python-trio/trio)
- error[invalid-return-type] src/trio/_core/_exceptions.py:120:14: Function can implicitly return `None`, which is not assignable to return type `Self`
+ error[invalid-return-type] src/trio/_core/_exceptions.py:120:14: Function always implicitly returns `None`, which is not assignable to return type `Self`
- error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:19:18: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:19:18: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:23:20: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:23:20: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:31:18: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:31:18: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/_file_io.py:304:57: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] src/trio/_file_io.py:304:57: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:306:61: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] src/trio/_file_io.py:306:61: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/trio/_file_io.py:310:64: Function can implicitly return `None`, which is not assignable to return type `T`
+ error[invalid-return-type] src/trio/_file_io.py:310:64: Function always implicitly returns `None`, which is not assignable to return type `T`
- error[invalid-return-type] src/trio/_file_io.py:312:57: Function can implicitly return `None`, which is not assignable to return type `BinaryIO`
+ error[invalid-return-type] src/trio/_file_io.py:312:57: Function always implicitly returns `None`, which is not assignable to return type `BinaryIO`
- error[invalid-return-type] src/trio/_file_io.py:314:51: Function can implicitly return `None`, which is not assignable to return type `RawIOBase`
+ error[invalid-return-type] src/trio/_file_io.py:314:51: Function always implicitly returns `None`, which is not assignable to return type `RawIOBase`
- error[invalid-return-type] src/trio/_file_io.py:316:72: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] src/trio/_file_io.py:316:72: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:318:59: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] src/trio/_file_io.py:318:59: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:320:53: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] src/trio/_file_io.py:320:53: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/trio/_file_io.py:322:53: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] src/trio/_file_io.py:322:53: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/trio/_file_io.py:324:57: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] src/trio/_file_io.py:324:57: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:325:57: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] src/trio/_file_io.py:325:57: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:326:61: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] src/trio/_file_io.py:326:61: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:327:61: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] src/trio/_file_io.py:327:61: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:328:61: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] src/trio/_file_io.py:328:61: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:329:69: Function can implicitly return `None`, which is not assignable to return type `AnyStr`
+ error[invalid-return-type] src/trio/_file_io.py:329:69: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_file_io.py:330:63: Function can implicitly return `None`, which is not assignable to return type `memoryview[Unknown]`
+ error[invalid-return-type] src/trio/_file_io.py:330:63: Function always implicitly returns `None`, which is not assignable to return type `memoryview[Unknown]`
- error[invalid-return-type] src/trio/_file_io.py:332:93: Function can implicitly return `None`, which is not assignable to return type `AnyStr`
+ error[invalid-return-type] src/trio/_file_io.py:332:93: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_file_io.py:333:87: Function can implicitly return `None`, which is not assignable to return type `bytes`
+ error[invalid-return-type] src/trio/_file_io.py:333:87: Function always implicitly returns `None`, which is not assignable to return type `bytes`
- error[invalid-return-type] src/trio/_file_io.py:334:73: Function can implicitly return `None`, which is not assignable to return type `AnyStr`
+ error[invalid-return-type] src/trio/_file_io.py:334:73: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_file_io.py:336:94: Function can implicitly return `None`, which is not assignable to return type `AnyStr`
+ error[invalid-return-type] src/trio/_file_io.py:336:94: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_file_io.py:337:77: Function can implicitly return `None`, which is not assignable to return type `list[AnyStr]`
+ error[invalid-return-type] src/trio/_file_io.py:337:77: Function always implicitly returns `None`, which is not assignable to return type `list[AnyStr]`
- error[invalid-return-type] src/trio/_file_io.py:338:92: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] src/trio/_file_io.py:338:92: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:339:59: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] src/trio/_file_io.py:339:59: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:340:95: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] src/trio/_file_io.py:340:95: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:341:76: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] src/trio/_file_io.py:341:76: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:343:88: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] src/trio/_file_io.py:343:88: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:344:85: Function can implicitly return `None`, which is not assignable to return type `AnyStr`
+ error[invalid-return-type] src/trio/_file_io.py:344:85: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_highlevel_ssl_helpers.py:120:6: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/trio/_highlevel_ssl_helpers.py:120:6: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/_socket.py:1120:59: Function can implicitly return `None`, which is not assignable to return type `Awaitable[bytes]`
+ error[invalid-return-type] src/trio/_socket.py:1120:59: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[bytes]`
- error[invalid-return-type] src/trio/_socket.py:1139:14: Function can implicitly return `None`, which is not assignable to return type `Awaitable[int]`
+ error[invalid-return-type] src/trio/_socket.py:1139:14: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[int]`
- error[invalid-return-type] src/trio/_socket.py:1157:14: Function can implicitly return `None`, which is not assignable to return type `Awaitable[tuple[bytes, @Todo(Support for `typing.TypeAlias`)]]`
+ error[invalid-return-type] src/trio/_socket.py:1157:14: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[tuple[bytes, @Todo(Support for `typing.TypeAlias`)]]`
- error[invalid-return-type] src/trio/_socket.py:1176:14: Function can implicitly return `None`, which is not assignable to return type `Awaitable[tuple[int, @Todo(Support for `typing.TypeAlias`)]]`
+ error[invalid-return-type] src/trio/_socket.py:1176:14: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[tuple[int, @Todo(Support for `typing.TypeAlias`)]]`
- error[invalid-return-type] src/trio/_socket.py:1198:18: Function can implicitly return `None`, which is not assignable to return type `Awaitable[tuple[bytes, list[tuple[int, int, bytes]], int, object]]`
+ error[invalid-return-type] src/trio/_socket.py:1198:18: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[tuple[bytes, list[tuple[int, int, bytes]], int, object]]`
- error[invalid-return-type] src/trio/_socket.py:1221:18: Function can implicitly return `None`, which is not assignable to return type `Awaitable[tuple[int, list[tuple[int, int, bytes]], int, object]]`
+ error[invalid-return-type] src/trio/_socket.py:1221:18: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[tuple[int, list[tuple[int, int, bytes]], int, object]]`
- error[invalid-return-type] src/trio/_socket.py:1235:61: Function can implicitly return `None`, which is not assignable to return type `Awaitable[int]`
+ error[invalid-return-type] src/trio/_socket.py:1235:61: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[int]`
- error[invalid-return-type] src/trio/_subprocess.py:51:44: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] src/trio/_subprocess.py:51:44: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_subprocess.py:830:14: Function can implicitly return `None`, which is not assignable to return type `Process`
+ error[invalid-return-type] src/trio/_subprocess.py:830:14: Function always implicitly returns `None`, which is not assignable to return type `Process`
- error[invalid-return-type] src/trio/_subprocess.py:893:14: Function can implicitly return `None`, which is not assignable to return type `CompletedProcess[bytes]`
+ error[invalid-return-type] src/trio/_subprocess.py:893:14: Function always implicitly returns `None`, which is not assignable to return type `CompletedProcess[bytes]`
- error[invalid-return-type] src/trio/_tests/pytest_plugin.py:50:56: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/trio/_tests/pytest_plugin.py:50:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/_util.py:355:10: Function can implicitly return `None`, which is not assignable to return type `(Fn, /) -> Fn`
+ error[invalid-return-type] src/trio/_util.py:355:10: Function always implicitly returns `None`, which is not assignable to return type `(Fn, /) -> Fn`
- error[invalid-return-type] src/trio/_util.py:381:6: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/trio/_util.py:381:6: Function always implicitly returns `None`, which is not assignable to return type `Never`

mkosi (https://github.com/systemd/mkosi)
- error[invalid-return-type] mkosi/log.py:20:57: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] mkosi/log.py:20:57: Function always implicitly returns `None`, which is not assignable to return type `Never`

Expression (https://github.com/cognitedata/Expression)
- error[invalid-return-type] expression/core/typing.py:53:73: Function can implicitly return `None`, which is not assignable to return type `tuple[Any, Any]`
+ error[invalid-return-type] expression/core/typing.py:53:73: Function always implicitly returns `None`, which is not assignable to return type `tuple[Any, Any]`

jinja (https://github.com/pallets/jinja)
- error[invalid-return-type] src/jinja2/ext.py:34:59: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] src/jinja2/ext.py:34:59: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/jinja2/ext.py:38:14: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] src/jinja2/ext.py:38:14: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/jinja2/parser.py:95:10: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/jinja2/parser.py:95:10: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/jinja2/parser.py:130:73: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/jinja2/parser.py:130:73: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/jinja2/parser.py:141:10: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/jinja2/parser.py:141:10: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/jinja2/runtime.py:948:14: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/jinja2/runtime.py:948:14: Function always implicitly returns `None`, which is not assignable to return type `Never`

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:42:42: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:42:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:45:41: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:45:41: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:48:41: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:48:41: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:51:56: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:51:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:54:38: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:54:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:57:38: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:57:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:60:42: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:60:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:63:51: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:63:51: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:66:41: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:66:41: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:69:33: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:69:33: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:72:66: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:72:66: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:117:64: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:117:64: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:120:57: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:120:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:123:40: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:123:40: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:126:57: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:126:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:129:26: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:129:26: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:132:56: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:132:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:135:42: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:135:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:138:24: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:138:24: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:156:48: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:156:48: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:159:30: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:159:30: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:162:38: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:162:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:165:55: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:165:55: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:168:73: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:168:73: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:185:59: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:185:59: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:188:56: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:188:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:191:68: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:191:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:194:53: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:194:53: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:197:68: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:197:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:200:75: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:200:75: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:203:37: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:203:37: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:206:57: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:206:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:209:57: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:209:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:212:40: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:212:40: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:215:51: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:215:51: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:218:68: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:218:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:221:26: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:221:26: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:224:57: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:224:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:227:61: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/datastructures/mixins.py:227:61: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/exceptions.py:875:69: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] src/werkzeug/exceptions.py:875:69: Function always implicitly returns `None`, which is not assignable to return type `Never`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] tests/annotations/behaviors.py:22:28: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/behaviors.py:22:28: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:451:33: Function can implicitly return `None`, which is not assignable to return type `HydraPartialBuilds[T]`
+ error[invalid-return-type] tests/annotations/declarations.py:451:33: Function always implicitly returns `None`, which is not assignable to return type `HydraPartialBuilds[T]`
- error[invalid-return-type] tests/annotations/declarations.py:467:16: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] tests/annotations/declarations.py:467:16: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] tests/annotations/declarations.py:469:22: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] tests/annotations/declarations.py:469:22: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] tests/annotations/declarations.py:796:29: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:796:29: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1072:26: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1072:26: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1085:27: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1085:27: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1097:34: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1097:34: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1146:30: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1146:30: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1149:31: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1149:31: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1161:23: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] tests/annotations/declarations.py:1161:23: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] tests/annotations/declarations.py:1164:23: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] tests/annotations/declarations.py:1164:23: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] tests/annotations/declarations.py:1174:32: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1174:32: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1253:24: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1253:24: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1287:24: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1287:24: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1322:24: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1322:24: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/declarations.py:1357:24: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/declarations.py:1357:24: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/mypy_checks.py:16:22: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/mypy_checks.py:16:22: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/mypy_checks.py:32:22: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/mypy_checks.py:32:22: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/mypy_checks.py:55:30: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/mypy_checks.py:55:30: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/mypy_checks.py:58:31: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/mypy_checks.py:58:31: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] tests/annotations/mypy_checks.py:71:23: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] tests/annotations/mypy_checks.py:71:23: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] tests/annotations/mypy_checks.py:74:23: Function can implicitly return `None`, which is not assignable to return type `bool`
+ error[invalid-return-type] tests/annotations/mypy_checks.py:74:23: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] tests/annotations/mypy_checks.py:87:32: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] tests/annotations/mypy_checks.py:87:32: Function always implicitly returns `None`, which is not assignable to return type `str`

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-return-type] psycopg/psycopg/errors.py:87:31: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:87:31: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:93:24: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:93:24: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:99:30: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:99:30: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:102:29: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:102:29: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:109:47: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:109:47: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:113:25: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:113:25: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:116:36: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:116:36: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:122:42: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:122:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:125:48: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:125:48: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:128:43: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:128:43: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:131:50: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:131:50: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:134:38: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:134:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:137:44: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:137:44: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:140:48: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:140:48: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:143:53: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:143:53: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:146:46: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:146:46: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:149:51: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:149:51: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:152:45: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:152:45: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:155:50: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:155:50: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:158:43: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:158:43: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:161:48: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:161:48: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:164:29: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:164:29: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:167:32: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:167:32: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:170:26: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:170:26: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:173:24: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:173:24: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:176:38: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:176:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:179:51: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:179:51: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:182:30: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:182:30: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:185:29: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:185:29: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:188:27: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:188:27: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:191:44: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:191:44: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:194:43: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:194:43: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:197:44: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:197:44: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:200:36: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:200:36: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:203:46: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:203:46: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:206:26: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:206:26: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:209:47: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:209:47: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:212:46: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:212:46: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:215:48: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:215:48: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:218:38: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:218:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:221:37: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:221:37: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:224:32: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:224:32: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] psycopg/psycopg/errors.py:227:37: Function can implicitly return `None`, which is not assignable to return type `Never`
+ error[invalid-return-type] psycopg/psycopg/errors.py:227:37: Function always implicitly returns `None`, which is not assignable to return type `Never`

operator (https://github.com/canonical/operator)
- error[invalid-return-type] ops/charm.py:1364:25: Function can implicitly return `None`, which is not assignable to return type `CharmEvents`
+ error[invalid-return-type] ops/charm.py:1364:25: Function always implicitly returns `None`, which is not assignable to return type `CharmEvents`
- error[invalid-return-type] ops/framework.py:394:25: Function can implicitly return `None`, which is not assignable to return type `ObjectEvents`
+ error[invalid-return-type] ops/framework.py:394:25: Function always implicitly returns `None`, which is not assignable to return type `ObjectEvents`
- error[invalid-return-type] ops/framework.py:606:25: Function can implicitly return `None`, which is not assignable to return type `FrameworkEvents`
+ error[invalid-return-type] ops/framework.py:606:25: Function always implicitly returns `None`, which is not assignable to return type `FrameworkEvents`
- error[invalid-return-type] ops/framework.py:1158:28: Function can implicitly return `None`, which is not assignable to return type `StoredStateData`
+ error[invalid-return-type] ops/framework.py:1158:28: Function always implicitly returns `None`, which is not assignable to return type `StoredStateData`
- error[invalid-return-type] ops/framework.py:1161:33: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] ops/framework.py:1161:33: Function always implicitly returns `None`, which is not assignable to return type `str`

comtypes (https://github.com/enthought/comtypes)
- error[invalid-return-type] comtypes/_post_coinit/misc.py:57:33: Function can implicitly return `None`, which is not assignable to return type `GUID`
+ error[invalid-return-type] comtypes/_post_coinit/misc.py:57:33: Function always implicitly returns `None`, which is not assignable to return type `GUID`
- error[invalid-return-type] comtypes/automation.py:816:39: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] comtypes/automation.py:816:39: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/connectionpoints.py:35:43: Function can implicitly return `None`, which is not assignable to return type `IEnumConnectionPoints`
+ error[invalid-return-type] comtypes/connectionpoints.py:35:43: Function always implicitly returns `None`, which is not assignable to return type `IEnumConnectionPoints`
- error[invalid-return-type] comtypes/connectionpoints.py:36:56: Function can implicitly return `None`, which is not assignable to return type `IConnectionPoint`
+ error[invalid-return-type] comtypes/connectionpoints.py:36:56: Function always implicitly returns `None`, which is not assignable to return type `IConnectionPoint`
- error[invalid-return-type] comtypes/connectionpoints.py:45:50: Function can implicitly return `None`, which is not assignable to return type `IConnectionPointContainer`
+ error[invalid-return-type] comtypes/connectionpoints.py:45:50: Function always implicitly returns `None`, which is not assignable to return type `IConnectionPointContainer`
- error[invalid-return-type] comtypes/connectionpoints.py:46:49: Function can implicitly return `None`, which is not assignable to return type `int`
+ error[invalid-return-type] comtypes/connectionpoints.py:46:49: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/connectionpoints.py:48:38: Function can implicitly return `None`, which is not assignable to return type `IEnumConnections`
+ error[invalid-return-type] comtypes/connectionpoints.py:48:38: Function always implicitly returns `None`, which is not assignable to return type `IEnumConnections`
- error[invalid-return-type] comtypes/connectionpoints.py:57:46: Function can implicitly return `None`, which is not assignable to return type `tuple[tagCONNECTDATA, int]`
+ error[invalid-return-type] comtypes/connectionpoints.py:57:46: Function always implicitly returns `None`, which is not assignable to return type `tuple[tagCONNECTDATA, int]`
- error[invalid-return-type] comtypes/connectionpoints.py:60:28: Function can implicitly return `None`, which is not assignable to return type `IEnumConnections`
+ error[invalid-return-type] comtypes/connectionpoints.py:60:28: Function always implicitly returns `None`, which is not assignable to return type `IEnumConnections`
- error[invalid-return-type] comtypes/connectionpoints.py:78:46: Function can implicitly return `None`, which is not assignable to return type `tuple[IConnectionPoint, int]`
+ error[invalid-return-type] comtypes/connectionpoints.py:78:46: Function always implicitly returns `None`, which is not assignable to return type `tuple[IConnectionPoint, int]`
- error[invalid-return-type] comtypes/connectionpoints.py:81:28: Function can implicitly return `None`, which is not assignable to return type `IEnumConnectionPoints`
+ error[invalid-return-type] comtypes/connectionpoints.py:81:28: Function always implicitly returns `None`, which is not assignable to return type `IEnumConnectionPoints`
- error[invalid-return-type] comtypes/errorinfo.py:51:30: Function can implicitly return `None`, which is not assignable to return type `GUID`
+ error[invalid-return-type] comtypes/errorinfo.py:51:30: Function always implicitly returns `None`, which is not assignable to return type `GUID`
- error[invalid-return-type] comtypes/errorinfo.py:52:32: Function can implicitly return `None`, which is not assignable to return type `str`
+ error[invalid-return-type] comtypes/errorinfo.py:52:32: Function always implicitly returns `Non...*[Comment body truncated]*

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:290 on 2025-05-29 00:28_

If we are snapshotting, then we usually don't include the error message text here; it's just one more thing that needs manual update if the message changes.
```suggestion
# error: [invalid-return-type]
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1697 on 2025-05-29 00:30_

```suggestion
            "Function always implicitly returns `None`, which is not assignable to return type `{}`",
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1700 on 2025-05-29 00:36_

Personally I'm not clear why we would assume it's the annotation that's wrong, rather than a `return` statement having been forgotten in the function body. @AlexWaygood I think this info hint was your suggestion; any comment?

(Also, minor wording nit, I think we should avoid describing the annotation as "your annotation" -- we don't know if the person who wrote it is the same person reading the diagnostic.)

```suggestion
        diag.info("Consider changing the return annotation to `-> None` or adding a `return` statement");
```

---

_@carljm approved on 2025-05-29 00:37_

Thanks, looks good! Approving modulo a few comments.

---

_@lipefree reviewed on 2025-05-29 09:34_

---

_Review comment by @lipefree on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:290 on 2025-05-29 09:34_

Oh I get it. Thank you for the explanation

---

_@AlexWaygood reviewed on 2025-05-29 10:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1700 on 2025-05-29 10:47_

> Personally I'm not clear why we would assume it's the annotation that's wrong, rather than a `return` statement having been forgotten in the function body. @AlexWaygood I think this info hint was your suggestion; any comment?

agree that it makes sense to give both hints, yes! https://github.com/astral-sh/ty/issues/538#issuecomment-2919023647

---

_Label `ty` added by @ntBre on 2025-05-29 22:36_

---

_Review requested from @carljm by @lipefree on 2025-05-29 22:55_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:287 on 2025-05-29 23:09_

```suggestion
the return statement), then we show a diagnostic hint that the return annotation should be
`-> None` or a `return` statement should be added.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1694 on 2025-05-29 23:09_

```suggestion
    // If no return statement is defined in the function, then the function always returns `None`
```

---

_@carljm approved on 2025-05-29 23:10_

---

_Merged by @carljm on 2025-05-29 23:17_

---

_Closed by @carljm on 2025-05-29 23:17_

---
