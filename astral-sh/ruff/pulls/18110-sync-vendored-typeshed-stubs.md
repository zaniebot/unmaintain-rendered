```yaml
number: 18110
title: Sync vendored typeshed stubs
type: pull_request
state: merged
author: github-actions
labels:
  - internal
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-05-15T00:29:53Z
updated_at: 2025-05-15T02:14:54Z
url: https://github.com/astral-sh/ruff/pull/18110
synced_at: 2026-01-12T15:56:12Z
```

# Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Label `internal` added by @github-actions[bot] on 2025-05-15 00:29_

---

_Review requested from @carljm by @github-actions[bot] on 2025-05-15 00:29_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-05-15 00:29_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-05-15 00:29_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-05-15 00:29_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-05-15 00:29_

---

_Label `internal` added by @github-actions[bot] on 2025-05-15 00:29_

---

_Closed by @AlexWaygood on 2025-05-15 00:33_

---

_Reopened by @AlexWaygood on 2025-05-15 00:33_

---

_Comment by @github-actions[bot] on 2025-05-15 00:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:127:5: Argument does not have asserted type `AsyncIOWrapper[BufferedReader]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:127:5: Argument does not have asserted type `AsyncIOWrapper[BufferedReader[_BufferedReaderStream]]`

urllib3 (https://github.com/urllib3/urllib3)
+ warning[unused-ignore-comment] test/test_response.py:770:36: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] test/test_response.py:782:41: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] test/test_response.py:787:39: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] test/test_response.py:800:40: Unused blanket `type: ignore` directive
- Found 464 diagnostics
+ Found 468 diagnostics

mypy (https://github.com/python/mypy)
- error[invalid-argument-type] mypy/fastparse.py:1376:36: Argument to bound method `__init__` is incorrect: Expected `str`, found `@Todo(Support for `typing.TypeAlias`) | None`
+ error[invalid-argument-type] mypy/fastparse.py:1376:36: Argument to bound method `__init__` is incorrect: Expected `str`, found `str | None`
+ error[unresolved-import] mypy/typeshed/stdlib/email/__init__.pyi:3:34: Module `email.policy` has no member `_MessageT`
+ error[unresolved-import] mypy/typeshed/stdlib/email/mime/message.pyi:2:34: Module `email.policy` has no member `_MessageT`
+ error[unresolved-import] mypy/typeshed/stdlib/email/mime/multipart.pyi:4:34: Module `email.policy` has no member `_MessageT`
- Found 3358 diagnostics
+ Found 3361 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[unsupported-operator] src/_pytest/assertion/rewrite.py:1097:34: Operator `+` is unsupported between objects of type `@Todo(Support for `typing.TypeAlias`) | None` and `Literal["="]`
+ error[unsupported-operator] src/_pytest/assertion/rewrite.py:1097:34: Operator `+` is unsupported between objects of type `str | None` and `Literal["="]`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[unresolved-attribute] tests/appsec/appsec/test_common_modules.py:36:16: Type `Overload[(file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`) = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> TextIOWrapper[_WrappedBuffer], (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> FileIO, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BufferedRandom, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BufferedWriter, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BufferedReader, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BinaryIO, (file: @Todo(Support for `typing.TypeAlias`), mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> IO[Any]]` has no attribute `__wrapped__`
+ error[unresolved-attribute] tests/appsec/appsec/test_common_modules.py:36:16: Type `Overload[(file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`) = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> TextIOWrapper[_WrappedBuffer], (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> FileIO, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BufferedRandom, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BufferedWriter, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BufferedReader[_BufferedReaderStream], (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BinaryIO, (file: @Todo(Support for `typing.TypeAlias`), mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> IO[Any]]` has no attribute `__wrapped__`

```
</details>


---

_Comment by @carljm on 2025-05-15 01:42_

I'm fixing this up. Mostly just insta snapshots from changed typeshed line numbers, but there is one odd failure related to the `__value__` attribute of a PEP 695 type alias.

---

_Comment by @AlexWaygood on 2025-05-15 01:50_

A bit of a perf regression due to the new 3.14 branches, but such is life: https://codspeed.io/astral-sh/ruff/branches/typeshedbot%2Fsync-typeshed

---

_Comment by @carljm on 2025-05-15 02:09_

The return value of `TypeAliasType.__value__` in typeshed is now `AnnotationForm`, which is a `TypeAlias` for `Any`, thus the new `@Todo` type in the PEP695 type alias tests.

---

_@AlexWaygood approved on 2025-05-15 02:12_

---

_Merged by @carljm on 2025-05-15 02:14_

---

_Closed by @carljm on 2025-05-15 02:14_

---

_Branch deleted on 2025-05-15 02:14_

---
