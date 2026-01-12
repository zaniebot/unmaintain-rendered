```yaml
number: 18050
title: "[ty] Improve diagnostics for `assert_type` and `assert_never`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/assert-type-range
created_at: 2025-05-12T17:05:06Z
updated_at: 2025-05-13T13:00:21Z
url: https://github.com/astral-sh/ruff/pull/18050
synced_at: 2026-01-12T15:56:11Z
```

# [ty] Improve diagnostics for `assert_type` and `assert_never`

---

_@AlexWaygood_

## Summary

Further work towards https://github.com/astral-sh/ty/issues/209.

I didn't make exactly the change suggested in that issue (only highlight the argument range) because I was worried that the behaviour of `type: ignore` comments might be confusing if the range of the diagnostic only covered part of the call. E.g. I'd expect this to work, as a user:

```py
assert_type(  # type: ignore
    very_very_very_long_variable_name,
    very_very_very_long_type,
)
```

I feel like in general we might want to separate the concepts of "range the diagnostic has for suppression purposes" and "primary range that is highlighted when the diagnostic is rendered on the terminal"? But that's out of scope for this PR.

Instead, this PR adds a subdiagnostic that draws the user's attention to the range of the argument passed in.

## Test Plan

Before:

![image](https://github.com/user-attachments/assets/a47fa90b-7b53-47c6-9955-bddf8fe0629e)

After:

![image](https://github.com/user-attachments/assets/9a94182e-4928-4f43-a00b-fce8b53ea8b4)

(I also added snapshots)


---

_Review requested from @carljm by @AlexWaygood on 2025-05-12 17:05_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-12 17:05_

---

_Label `ty` added by @AlexWaygood on 2025-05-12 17:05_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-12 17:05_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-12 17:05_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-05-12 17:05_

---

_Comment by @github-actions[bot] on 2025-05-12 17:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- error[type-assertion-failure] tests/type_tests.py:21:9: Actual type `Never` is not the same as asserted type `() -> None`
+ error[type-assertion-failure] tests/type_tests.py:21:9: Argument does not have asserted type `() -> None`
- error[type-assertion-failure] tests/type_tests.py:27:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> None`
+ error[type-assertion-failure] tests/type_tests.py:27:9: Argument does not have asserted type `() -> None`
- error[type-assertion-failure] tests/type_tests.py:33:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> Never`
+ error[type-assertion-failure] tests/type_tests.py:33:9: Argument does not have asserted type `() -> Never`
- error[type-assertion-failure] tests/type_tests.py:41:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> AbstractContextManager[None, bool | None]`
+ error[type-assertion-failure] tests/type_tests.py:41:9: Argument does not have asserted type `() -> AbstractContextManager[None, bool | None]`
- error[type-assertion-failure] tests/type_tests.py:49:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> _GeneratorContextManager[None, None, None]`
+ error[type-assertion-failure] tests/type_tests.py:49:9: Argument does not have asserted type `() -> _GeneratorContextManager[None, None, None]`
- error[type-assertion-failure] tests/type_tests.py:57:9: Actual type `(...) -> Unknown` is not the same as asserted type `Never`
+ error[type-assertion-failure] tests/type_tests.py:57:9: Argument does not have asserted type `Never`

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
- error[type-assertion-failure] test/test_generated_mypy.py:485:5: Actual type `Unknown` is not the same as asserted type `Email`
+ error[type-assertion-failure] test/test_generated_mypy.py:485:5: Argument does not have asserted type `Email`
- error[type-assertion-failure] test/test_generated_mypy.py:486:5: Actual type `Unknown` is not the same as asserted type `ScalarMap[Unknown, Email]`
+ error[type-assertion-failure] test/test_generated_mypy.py:486:5: Argument does not have asserted type `ScalarMap[Unknown, Email]`
- error[type-assertion-failure] test/test_generated_mypy.py:496:5: Actual type `Unknown` is not the same as asserted type `Email`
+ error[type-assertion-failure] test/test_generated_mypy.py:496:5: Argument does not have asserted type `Email`
- error[type-assertion-failure] test/test_generated_mypy.py:497:5: Actual type `Unknown` is not the same as asserted type `ScalarMap[Unknown, Email]`
+ error[type-assertion-failure] test/test_generated_mypy.py:497:5: Argument does not have asserted type `ScalarMap[Unknown, Email]`

starlette (https://github.com/encode/starlette)
- error[type-assertion-failure] tests/test_config.py:21:5: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/test_config.py:21:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/test_config.py:23:5: Actual type `None` is not the same as asserted type `str | None`
+ error[type-assertion-failure] tests/test_config.py:23:5: Argument does not have asserted type `str | None`
- error[type-assertion-failure] tests/test_config.py:24:5: Actual type `Literal[""]` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/test_config.py:24:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/test_config.py:26:5: Actual type `Unknown` is not the same as asserted type `bool`
+ error[type-assertion-failure] tests/test_config.py:26:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] tests/test_config.py:27:5: Actual type `Literal[False]` is not the same as asserted type `bool`
+ error[type-assertion-failure] tests/test_config.py:27:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] tests/test_config.py:28:5: Actual type `None` is not the same as asserted type `bool | None`
+ error[type-assertion-failure] tests/test_config.py:28:5: Argument does not have asserted type `bool | None`

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[type-assertion-failure] strawberry/channels/handlers/http_handler.py:221:17: Expected type `Never`, got `@Todo(generic `typing.Awaitable` type) & ~MultipartChannelsResponse & ~ChannelsResponse` instead
+ error[type-assertion-failure] strawberry/channels/handlers/http_handler.py:221:17: Argument does not have asserted type `Never`

trio (https://github.com/python-trio/trio)
- error[type-assertion-failure] src/trio/_core/_tests/type_tests/run.py:39:1: Actual type `Unknown` is not the same as asserted type `list[int | float]`
+ error[type-assertion-failure] src/trio/_core/_tests/type_tests/run.py:39:1: Argument does not have asserted type `list[int | float]`
- error[type-assertion-failure] src/trio/_core/_tests/type_tests/run.py:44:1: Actual type `Unknown` is not the same as asserted type `int`
+ error[type-assertion-failure] src/trio/_core/_tests/type_tests/run.py:44:1: Argument does not have asserted type `int`
- error[type-assertion-failure] src/trio/_core/_tests/type_tests/run.py:50:1: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] src/trio/_core/_tests/type_tests/run.py:50:1: Argument does not have asserted type `str`
- error[type-assertion-failure] src/trio/_core/_tests/type_tests/run.py:51:1: Actual type `Unknown` is not the same as asserted type `int`
+ error[type-assertion-failure] src/trio/_core/_tests/type_tests/run.py:51:1: Argument does not have asserted type `int`
- error[type-assertion-failure] src/trio/_tests/type_tests/check_wraps.py:9:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `int`
+ error[type-assertion-failure] src/trio/_tests/type_tests/check_wraps.py:9:5: Argument does not have asserted type `int`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:16:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:16:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:17:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:17:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:18:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:18:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:19:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:19:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:42:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:42:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:43:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:43:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:53:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:53:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:55:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:55:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:57:9: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:57:9: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:58:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:58:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:59:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:59:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:60:5: Actual type `Unknown` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:60:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:64:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:64:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:65:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:65:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:66:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `stat_result`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:66:5: Argument does not have asserted type `stat_result`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:67:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:67:5: Argument does not have asserted type `None`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:68:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:68:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:69:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:69:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:71:9: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:71:9: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:73:9: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `str`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:73:9: Argument does not have asserted type `str`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:74:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:74:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:75:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:75:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:77:9: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:77:9: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:79:9: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:79:9: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:80:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:80:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:81:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:81:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:82:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:82:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:83:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:83:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:84:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:84:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:86:9: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:86:9: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:88:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:88:5: Argument does not have asserted type `None`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:89:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `stat_result`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:89:5: Argument does not have asserted type `stat_result`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:90:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:90:5: Argument does not have asserted type `None`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:93:9: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `str`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:93:9: Argument does not have asserted type `str`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:94:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bytes`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:94:5: Argument does not have asserted type `bytes`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:95:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `str`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:95:5: Argument does not have asserted type `str`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:96:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:96:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:97:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:97:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:98:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:98:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:99:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:99:5: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:101:9: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:101:9: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:103:9: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `Path`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:103:9: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:104:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:104:5: Argument does not have asserted type `None`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:105:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bool`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:105:5: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:106:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:106:5: Argument does not have asserted type `None`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:108:9: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:108:9: Argument does not have asserted type `None`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:109:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:109:5: Argument does not have asserted type `None`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:110:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:110:5: Argument does not have asserted type `None`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:111:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `int`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:111:5: Argument does not have asserted type `int`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:112:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `int`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:112:5: Argument does not have asserted type `int`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:120:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[TextIOWrapper[_WrappedBuffer]]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:120:5: Argument does not have asserted type `AsyncIOWrapper[TextIOWrapper[_WrappedBuffer]]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:121:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[TextIOWrapper[_WrappedBuffer]]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:121:5: Argument does not have asserted type `AsyncIOWrapper[TextIOWrapper[_WrappedBuffer]]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:122:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[TextIOWrapper[_WrappedBuffer]]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:122:5: Argument does not have asserted type `AsyncIOWrapper[TextIOWrapper[_WrappedBuffer]]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:123:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[TextIOWrapper[_WrappedBuffer]]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:123:5: Argument does not have asserted type `AsyncIOWrapper[TextIOWrapper[_WrappedBuffer]]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:124:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[FileIO]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:124:5: Argument does not have asserted type `AsyncIOWrapper[FileIO]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:125:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[BufferedRandom]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:125:5: Argument does not have asserted type `AsyncIOWrapper[BufferedRandom]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:126:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[BufferedWriter]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:126:5: Argument does not have asserted type `AsyncIOWrapper[BufferedWriter]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:127:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[BufferedReader]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:127:5: Argument does not have asserted type `AsyncIOWrapper[BufferedReader]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:128:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[BinaryIO]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:128:5: Argument does not have asserted type `AsyncIOWrapper[BinaryIO]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:129:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `AsyncIOWrapper[IO[Any]]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:129:5: Argument does not have asserted type `AsyncIOWrapper[IO[Any]]`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:133:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `bytes`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:133:5: Argument does not have asserted type `bytes`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:134:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `int`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:134:5: Argument does not have asserted type `int`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:135:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `int`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:135:5: Argument does not have asserted type `int`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:138:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `str`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:138:5: Argument does not have asserted type `str`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:139:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `int`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:139:5: Argument does not have asserted type `int`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:141:5: Actual type `@Todo(generic `typing.Awaitable` type)` is not the same as asserted type `list[str]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/path.py:141:5: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:25:5: Actual type `Never` is not the same as asserted type `ExceptionGroup[ValueError]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:25:5: Argument does not have asserted type `ExceptionGroup[ValueError]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:32:9: Actual type `ExceptionGroup[ValueError] | ValueError` is not the same as asserted type `ExceptionGroup[ValueError]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:32:9: Argument does not have asserted type `ExceptionGroup[ValueError]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:36:9: Actual type `ExceptionGroup[ValueError] | ValueError` is not the same as asserted type `BaseExceptionGroup[KeyboardInterrupt]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:36:9: Argument does not have asserted type `BaseExceptionGroup[KeyboardInterrupt]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:48:9: Actual type `BaseExceptionGroup[KeyboardInterrupt]` is not the same as asserted type `ExceptionGroup[ValueError]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:48:9: Argument does not have asserted type `ExceptionGroup[ValueError]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:128:5: Actual type `@Todo(unknown type subscript)` is not the same as asserted type `ExceptionGroup[ValueError]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:128:5: Argument does not have asserted type `ExceptionGroup[ValueError]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:137:5: Actual type `Never` is not the same as asserted type `ExceptionGroup[ExceptionGroup[ValueError]]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:137:5: Argument does not have asserted type `ExceptionGroup[ExceptionGroup[ValueError]]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:142:5: Actual type `Never` is not the same as asserted type `ExceptionGroup[ValueError] | ExceptionGroup[ExceptionGroup[ValueError]]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:142:5: Argument does not have asserted type `ExceptionGroup[ValueError] | ExceptionGroup[ExceptionGroup[ValueError]]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:218:9: Actual type `@Todo(unknown type subscript)` is not the same as asserted type `ExceptionGroup[ExceptionGroup[ExceptionGroup[ValueError]]]`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:218:9: Argument does not have asserted type `ExceptionGroup[ExceptionGroup[ExceptionGroup[ValueError]]]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:223:5: Actual type `Unknown | ((BaseExceptionGroup[Unknown], /) -> bool) | None` is not the same as asserted type `((BaseExceptionGroup[ValueError], /) -> bool) | None`
+ error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:223:5: Argument does not have asserted type `((BaseExceptionGroup[ValueError], /) -> bool) | None`

pydantic (https://github.com/pydantic/pydantic)
- error[type-assertion-failure] pydantic/json_schema.py:1426:13: Expected type `Never`, got `Unknown & ~str & ~list[Unknown]` instead
+ error[type-assertion-failure] pydantic/json_schema.py:1426:13: Argument does not have asserted type `Never`
- error[type-assertion-failure] pydantic/json_schema.py:1624:13: Expected type `Never`, got `Unknown` instead
+ error[type-assertion-failure] pydantic/json_schema.py:1624:13: Argument does not have asserted type `Never`

asynq (https://github.com/quora/asynq)
- error[type-assertion-failure] asynq/tests/test_typing.py:16:9: Actual type `Unknown` is not the same as asserted type `FutureBase[str]`
+ error[type-assertion-failure] asynq/tests/test_typing.py:16:9: Argument does not have asserted type `FutureBase[str]`

mkdocs (https://github.com/mkdocs/mkdocs)
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:72:9: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:72:9: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:95:9: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:95:9: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:114:9: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:114:9: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:122:9: Actual type `Unknown` is not the same as asserted type `str | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:122:9: Argument does not have asserted type `str | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:136:9: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:136:9: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:300:9: Actual type `Unknown` is not the same as asserted type `_IpAddressValue`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:300:9: Argument does not have asserted type `_IpAddressValue`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:301:9: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:301:9: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:302:9: Actual type `Unknown` is not the same as asserted type `int`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:302:9: Argument does not have asserted type `int`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:401:9: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:401:9: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:461:9: Actual type `Unknown` is not the same as asserted type `str | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:461:9: Argument does not have asserted type `str | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:490:9: Actual type `Unknown` is not the same as asserted type `str | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:490:9: Argument does not have asserted type `str | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:491:9: Actual type `Unknown` is not the same as asserted type `str | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:491:9: Argument does not have asserted type `str | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:533:9: Actual type `Unknown` is not the same as asserted type `str | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:533:9: Argument does not have asserted type `str | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:587:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:587:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:612:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:612:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:636:9: Actual type `Unknown` is not the same as asserted type `list[Unknown] | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:636:9: Argument does not have asserted type `list[Unknown] | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:650:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:650:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:694:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:694:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:712:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:712:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:753:9: Actual type `Unknown` is not the same as asserted type `dict[Unknown, Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:753:9: Argument does not have asserted type `dict[Unknown, Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:778:9: Actual type `Unknown` is not the same as asserted type `dict[Unknown, Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:778:9: Argument does not have asserted type `dict[Unknown, Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:802:9: Actual type `Unknown` is not the same as asserted type `dict[Unknown, Unknown] | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:802:9: Argument does not have asserted type `dict[Unknown, Unknown] | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:816:9: Actual type `Unknown` is not the same as asserted type `dict[Unknown, Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:816:9: Argument does not have asserted type `dict[Unknown, Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:862:17: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:862:17: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:874:17: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:874:17: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:886:17: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:886:17: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1017:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1017:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1043:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1043:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1124:9: Actual type `Unknown` is not the same as asserted type `Theme`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1124:9: Argument does not have asserted type `Theme`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1125:9: Actual type `Unknown` is not the same as asserted type `str | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1125:9: Argument does not have asserted type `str | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1440:9: Actual type `Unknown` is not the same as asserted type `Sub`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1440:9: Argument does not have asserted type `Sub`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1442:9: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1442:9: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1462:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1462:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1464:9: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1464:9: Argument does not have asserted type `str`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1481:9: Actual type `Unknown` is not the same as asserted type `list[Unknown] | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1481:9: Argument does not have asserted type `list[Unknown] | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1484:9: Actual type `Unknown` is not the same as asserted type `int | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1484:9: Argument does not have asserted type `int | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1513:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1513:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1515:9: Actual type `Unknown` is not the same as asserted type `int`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1515:9: Argument does not have asserted type `int`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1542:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1542:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1669:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1669:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1670:9: Actual type `Unknown` is not the same as asserted type `dict[Unknown, Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1670:9: Argument does not have asserted type `dict[Unknown, Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1971:9: Actual type `Unknown` is not the same as asserted type `PluginCollection`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1971:9: Argument does not have asserted type `PluginCollection`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1976:9: Actual type `Unknown` is not the same as asserted type `BasePlugin[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:1976:9: Argument does not have asserted type `BasePlugin[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:2405:9: Actual type `Unknown` is not the same as asserted type `int`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:2405:9: Argument does not have asserted type `int`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:2407:9: Actual type `Unknown` is not the same as asserted type `str | None`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:2407:9: Argument does not have asserted type `str | None`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:2409:9: Actual type `Unknown` is not the same as asserted type `list[Unknown]`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:2409:9: Argument does not have asserted type `list[Unknown]`
- error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:2418:9: Actual type `Unknown` is not the same as asserted type `int`
+ error[type-assertion-failure] mkdocs/tests/config/config_options_tests.py:2418:9: Argument does not have asserted type `int`
- error[type-assertion-failure] mkdocs/tests/plugin_tests.py:78:9: Actual type `Unknown` is not the same as asserted type `int`
+ error[type-assertion-failure] mkdocs/tests/plugin_tests.py:78:9: Argument does not have asserted type `int`
- error[type-assertion-failure] mkdocs/tests/plugin_tests.py:80:9: Actual type `Unknown | None` is not the same as asserted type `str | None`
+ error[type-assertion-failure] mkdocs/tests/plugin_tests.py:80:9: Argument does not have asserted type `str | None`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[type-assertion-failure] mitmproxy/addons/next_layer.py:338:17: Expected type `Never`, got `@Todo(Inference of subscript on special form)` instead
- error[type-assertion-failure] mitmproxy/addons/next_layer.py:402:17: Expected type `Never`, got `Literal["http", "https", "http3", "tls", "dtls", "tcp", "udp", "dns", "quic"]` instead
+ error[type-assertion-failure] mitmproxy/addons/next_layer.py:338:17: Argument does not have asserted type `Never`
+ error[type-assertion-failure] mitmproxy/addons/next_layer.py:402:17: Argument does not have asserted type `Never`
- error[type-assertion-failure] mitmproxy/contentviews/_utils.py:50:13: Expected type `Never`, got `@Todo(`match` pattern definition types)` instead
+ error[type-assertion-failure] mitmproxy/contentviews/_utils.py:50:13: Argument does not have asserted type `Never`
- error[type-assertion-failure] mitmproxy/net/http/http1/read.py:126:17: Expected type `Never`, got `@Todo(`match` pattern definition types)` instead
+ error[type-assertion-failure] mitmproxy/net/http/http1/read.py:126:17: Argument does not have asserted type `Never`
- error[type-assertion-failure] mitmproxy/net/http/validate.py:131:17: Expected type `Never`, got `@Todo(`match` pattern definition types)` instead
+ error[type-assertion-failure] mitmproxy/net/http/validate.py:131:17: Argument does not have asserted type `Never`
- error[type-assertion-failure] mitmproxy/proxy/layers/http/_events.py:127:17: Expected type `Never`, got `@Todo(`match` pattern definition types)` instead
+ error[type-assertion-failure] mitmproxy/proxy/layers/http/_events.py:127:17: Argument does not have asserted type `Never`
- error[type-assertion-failure] mitmproxy/proxy/layers/http/_http2.py:186:33: Expected type `Never`, got `@Todo(`match` pattern definition types)` instead
+ error[type-assertion-failure] mitmproxy/proxy/layers/http/_http2.py:186:33: Argument does not have asserted type `Never`
- error[type-assertion-failure] mitmproxy/proxy/layers/http/_http3.py:127:33: Expected type `Never`, got `@Todo(`match` pattern definition types)` instead
+ error[type-assertion-failure] mitmproxy/proxy/layers/http/_http3.py:127:33: Argument does not have asserted type `Never`
- error[type-assertion-failure] mitmproxy/proxy/layers/modes.py:82:17: Expected type `Never`, got `(Unknown & Literal["http"]) | (Unknown & Literal["https"]) | (Unknown & Literal["http3"]) | (Unknown & Literal["tls"]) | (Unknown & Literal["dtls"]) | (Unknown & Literal["tcp"]) | (Unknown & Literal["udp"]) | (Unknown & Literal["dns"]) | (Unknown & Literal["quic"])` instead
+ error[type-assertion-failure] mitmproxy/proxy/layers/modes.py:82:17: Argument does not have asserted type `Never`
- error[type-assertion-failure] test/mitmproxy/addons/test_dns_resolver.py:155:17: Expected type `Never`, got `@Todo(`match` pattern definition types)` instead
+ error[type-assertion-failure] test/mitmproxy/addons/test_dns_resolver.py:155:17: Argument does not have asserted type `Never`

pytest (https://github.com/pytest-dev/pytest)
- error[type-assertion-failure] testing/typing_checks.py:55:5: Actual type `ExceptionInfo[BaseException] | Unknown` is not the same as asserted type `ExceptionInfo[RuntimeError] | None`
+ error[type-assertion-failure] testing/typing_checks.py:55:5: Argument does not have asserted type `ExceptionInfo[RuntimeError] | None`
- error[type-assertion-failure] testing/typing_raises_group.py:39:5: Actual type `Never` is not the same as asserted type `@Todo(unknown type subscript)`
+ error[type-assertion-failure] testing/typing_raises_group.py:39:5: Argument does not have asserted type `@Todo(unknown type subscript)`
- error[type-assertion-failure] testing/typing_raises_group.py:155:5: Actual type `Never` is not the same as asserted type `@Todo(unknown type subscript)`
+ error[type-assertion-failure] testing/typing_raises_group.py:155:5: Argument does not have asserted type `@Todo(unknown type subscript)`
- error[type-assertion-failure] testing/typing_raises_group.py:160:5: Actual type `Never` is not the same as asserted type `@Todo(unknown type subscript)`
+ error[type-assertion-failure] testing/typing_raises_group.py:160:5: Argument does not have asserted type `@Todo(unknown type subscript)`
- error[type-assertion-failure] testing/typing_raises_group.py:241:5: Actual type `Unknown | ((@Todo(unknown type subscript), /) -> bool) | None` is not the same as asserted type `((...) -> bool) | None`
+ error[type-assertion-failure] testing/typing_raises_group.py:241:5: Argument does not have asserted type `((...) -> bool) | None`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[type-assertion-failure] tests/annotations/declarations.py:969:5: Actual type `FullBuilds[Unknown] | PBuilds[Unknown] | StdBuilds[Unknown]` is not the same as asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/annotations/declarations.py:980:5: Actual type `FullBuilds[Unknown] | PBuilds[Unknown] | StdBuilds[Unknown]` is not the same as asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:951:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:969:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:980:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/annotations/declarations.py:1062:5: Actual type `Path` is not the same as asserted type `Any`
+ error[type-assertion-failure] tests/annotations/declarations.py:1062:5: Argument does not have asserted type `Any`
- error[type-assertion-failure] tests/annotations/declarations.py:1063:5: Actual type `Path` is not the same as asserted type `Any`
+ error[type-assertion-failure] tests/annotations/declarations.py:1063:5: Argument does not have asserted type `Any`
- error[type-assertion-failure] tests/annotations/declarations.py:1064:5: Actual type `Path` is not the same as asserted type `Any`
+ error[type-assertion-failure] tests/annotations/declarations.py:1064:5: Argument does not have asserted type `Any`
- error[type-assertion-failure] tests/annotations/declarations.py:1065:5: Actual type `Path` is not the same as asserted type `Any`
+ error[type-assertion-failure] tests/annotations/declarations.py:1065:5: Argument does not have asserted type `Any`
- error[type-assertion-failure] tests/annotations/declarations.py:1066:5: Actual type `Path` is not the same as asserted type `Any`
+ error[type-assertion-failure] tests/annotations/declarations.py:1066:5: Argument does not have asserted type `Any`
- error[type-assertion-failure] tests/annotations/declarations.py:1067:5: Actual type `Path` is not the same as asserted type `Any`
+ error[type-assertion-failure] tests/annotations/declarations.py:1067:5: Argument does not have asserted type `Any`
- error[type-assertion-failure] tests/annotations/declarations.py:1074:5: Actual type `@Todo(specialized non-generic class)` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/declarations.py:1074:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/declarations.py:1075:5: Actual type `@Todo(specialized non-generic class)` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/declarations.py:1075:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/declarations.py:1076:5: Actual type `@Todo(specialized non-generic class)` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/declarations.py:1076:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/declarations.py:1087:5: Actual type `@Todo(specialized non-generic class)` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/declarations.py:1087:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/declarations.py:1088:5: Actual type `@Todo(specialized non-generic class)` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/declarations.py:1088:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/declarations.py:1089:5: Actual type `@Todo(specialized non-generic class)` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/declarations.py:1089:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/declarations.py:1208:5: Actual type `dict[Unknown, Unknown]` is not the same as asserted type `dict[str, int]`
+ error[type-assertion-failure] tests/annotations/declarations.py:1208:5: Argument does not have asserted type `dict[str, int]`
- error[type-assertion-failure] tests/annotations/declarations.py:1211:5: Actual type `dict[tuple[@Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.TypeAlias`)], @Todo(Support for `typing.TypeAlias`)]` is not the same as asserted type `dict[tuple[str | None, str], Any]`
+ error[type-assertion-failure] tests/annotations/declarations.py:1211:5: Argument does not have asserted type `dict[tuple[str | None, str], Any]`
- error[type-assertion-failure] tests/annotations/declarations.py:1402:5: Actual type `PBuilds[Unknown]` is not the same as asserted type `PBuilds[int]`
+ error[type-assertion-failure] tests/annotations/declarations.py:1402:5: Argument does not have asserted type `PBuilds[int]`
- error[type-assertion-failure] tests/annotations/declarations.py:1409:5: Actual type `FullBuilds[Unknown]` is not the same as asserted type `FullBuilds[int]`
+ error[type-assertion-failure] tests/annotations/declarations.py:1409:5: Argument does not have asserted type `FullBuilds[int]`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:18:5: Actual type `Any` is not the same as asserted type `A`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:18:5: Argument does not have asserted type `A`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:19:5: Actual type `Any` is not the same as asserted type `A`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:19:5: Argument does not have asserted type `A`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:20:5: Actual type `Any` is not the same as asserted type `A`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:20:5: Argument does not have asserted type `A`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:22:5: Actual type `Any` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:22:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:23:5: Actual type `Any` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:23:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:24:5: Actual type `Any` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:24:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:42:5: Actual type `Any` is not the same as asserted type `A`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:42:5: Argument does not have asserted type `A`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:43:5: Actual type `Any` is not the same as asserted type `A`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:43:5: Argument does not have asserted type `A`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:44:5: Actual type `Any` is not the same as asserted type `A`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:44:5: Argument does not have asserted type `A`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:46:5: Actual type `Any` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:46:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:47:5: Actual type `Any` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:47:5: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/annotations/mypy_checks.py:48:5: Actual type `Any` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/annotations/mypy_checks.py:48:5: Argument does not have asserted type `str`
- Found 647 diagnostics
+ Found 650 diagnostics

mypy (https://github.com/python/mypy)
- error[type-assertion-failure] mypy/checkexpr.py:3572:17: Expected type `Never`, got `Unknown` instead
+ error[type-assertion-failure] mypy/checkexpr.py:3572:17: Argument does not have asserted type `Never`

django-stubs (https://github.com/typeddjango/django-stubs)
- error[type-assertion-failure] tests/assert_type/apps/test_config.py:35:1: Actual type `Unknown | Literal["django.db.models.BigAutoField"]` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/assert_type/apps/test_config.py:35:1: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/assert_type/apps/test_config.py:37:1: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/assert_type/apps/test_config.py:37:1: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/assert_type/apps/test_config.py:38:1: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/assert_type/apps/test_config.py:38:1: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:74:1: Actual type `list[str]` is not the same as asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:74:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:75:1: Actual type `list[str]` is not the same as asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:75:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:76:1: Actual type `list[str]` is not the same as asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:76:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:77:1: Actual type `list[str]` is not the same as asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/contrib/admin/test_utils.py:77:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/assert_type/contrib/auth/test_models.py:13:1: Actual type `Unknown` is not the same as asserted type `MyUser`
+ error[type-assertion-failure] tests/assert_type/contrib/auth/test_models.py:13:1: Argument does not have asserted type `MyUser`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:15:1: Actual type `Unknown` is not the same as asserted type `list[str]`
+ error[type-assertion-failure] tests/assert_type/db/models/_enums.py:15:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:16:1: Actual type `Unknown` is not the same as asserted type `list[str]`
+ error[type-assertion-failure] tests/assert_type/db/models/_enums.py:16:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:17:1: Actual type `Unknown` is not the same as asserted type `list[str]`
+ error[type-assertion-failure] tests/assert_type/db/models/_enums.py:17:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:18:1: Actual type `Unknown` is not the same as asserted type `list[tuple[str, str]]`
+ error[type-assertion-failure] tests/assert_type/db/models/_enums.py:18:1: Argument does not have asserted type `list[tuple[str, str]]`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:20:1: Actual type `Unknown` is not the same as asserted type `Literal["NORTH"]`
+ error[type-assertion-failure] tests/assert_type/db/models/_enums.py:20:1: Argument does not have asserted type `Literal["NORTH"]`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:21:1: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/assert_type/db/models/_enums.py:21:1: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:22:1: Actual type `Unknown` is not the same as asserted type `str`
+ error[type-assertion-failure] tests/assert_type/db/models/_enums.py:22:1: Argument does not have asserted type `str`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:23:1: Actual type `Unknown` is not the same as asserted type `Literal[True]`
+ error[type-assertion-failure] tests/assert_type/db/models/_enums.py:23:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:127:1: Actual type `Unknown` is not the same as asserted type `list[str]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:127:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:128:1: Actual type `Unknown` is not the same as asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:128:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:129:1: Actual type `Unknown` is not the same as asserted type `list[int]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:129:1: Argument does not have asserted type `list[int]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:130:1: Actual type `Unknown` is not the same as asserted type `list[tuple[int, @Todo(Support for `typing.TypeAlias`)]]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:130:1: Argument does not have asserted type `list[tuple[int, @Todo(Support for `typing.TypeAlias`)]]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:132:1: Actual type `Unknown` is not the same as asserted type `Literal["CLUB"]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:132:1: Argument does not have asserted type `Literal["CLUB"]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:134:1: Actual type `Unknown` is not the same as asserted type `int`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:134:1: Argument does not have asserted type `int`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:135:1: Actual type `Unknown` is not the same as asserted type `Literal[True]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:135:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:138:1: Actual type `Unknown` is not the same as asserted type `list[str]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:138:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:139:1: Actual type `Unknown` is not the same as asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:139:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:140:1: Actual type `Unknown` is not the same as asserted type `list[str]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:140:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:141:1: Actual type `Unknown` is not the same as asserted type `list[tuple[str, @Todo(Support for `typing.TypeAlias`)]]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:141:1: Argument does not have asserted type `list[tuple[str, @Todo(Support for `typing.TypeAlias`)]]`
- error[type-assertion-failure] tests/assert_type/db/models/...*[Comment body truncated]*

---

_Comment by @MichaReiser on 2025-05-12 17:10_

Thanks for looking into this.

I think I'd prefer if we don't repeat the inferred type in the sub diagnostic message and diagnostic. I'm leaning towards only having it in the annotation but I'm not sure if that's possible.

The other issue that I see is that the actual type is now omitted when using `concise`. I'm not sure if that's a problem. I think that's an issue that we already have elsewhere and sort of the point of concise.

---

_Comment by @AlexWaygood on 2025-05-12 17:20_

Hmm, your feedback points contradict each other @MichaReiser 😆

> I think I'd prefer if we don't repeat the inferred type in the sub diagnostic message and diagnostic. I'm leaning towards only having it in the annotation but I'm not sure if that's possible.

I agree the duplication seems unfortunate, but I was worried that doing what you suggest here would make the diagnostic summary (which is what is displayed for the concise output format) pretty useless. I do think we should care about what `--output-format=concise` gives us, since:
1. I find that the verbose error format makes the diagnostics pretty overwhelming when running it on a codebase with lots of errors (and that will be the initial experience for most of our first-time users!); I find `--output-format=concise` much easier to sift through when dealing with a large volume of things.
2. When using rust-analyzer in VSCode, the diagnostic summary is all that's printed in the "problems" tab that lists all LSP diagnostics currently reported on your code. You can generally double-click on them to get the full diagnostic rendered in a separate window, but it would be pretty tedious if I had to do that with every single diagnostic in order to get enough information to understand what the issue was.

> The other issue that I see is that the actual type is now omitted when using `concise`. I'm not sure if that's a problem. I think that's an issue that we already have elsewhere and sort of the point of concise.

Yeah, I agree it's not ideal (see above). We do already have this issue for lots of other diagnostic, as you say, though.

---

_Comment by @MichaReiser on 2025-05-12 17:32_

> I agree the duplication seems unfortunate, but I was worried that doing what you suggest here would make the diagnostic summary (which is what is displayed for the concise output format) pretty useless. I do think we should care about what --output-format=concise gives us, since:

I don't think it changes the concise output because the concise output only displays information from the primary diagnostic, not from the secondary diagnostic (as you can see in the mdtests)

> When using rust-analyzer in VSCode, the diagnostic summary is all that's printed in the "problems" tab that lists all LSP diagnostics currently reported on your code. 

We can improve this. Again, the actual type isn't part of the primary message. 

That's why I think we shouldn't repeat the actual type in the sub-diagnostic either by removing it from the annotation or from the message (I'd prefer that)




---

_Comment by @AlexWaygood on 2025-05-12 17:34_

@MichaReiser what would you think about this change (relative to my PR):

<details>
<summary>Diff</summary>

```diff
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -4814,14 +4814,8 @@ impl<'db> TypeInferenceBuilder<'db> {
                                                             asserted_ty.display(self.db())
                                                         ));
 
-                                                    let mut subdiagnostic = SubDiagnostic::new(
-                                                        Severity::Info,
-                                                        format_args!(
-                                                            "`{}` is not an equivalent type to `{}`",
-                                                            actual_ty.display(self.db()),
-                                                            asserted_ty.display(self.db())
-                                                        ),
-                                                    );
+                                                    let mut subdiagnostic =
+                                                        SubDiagnostic::new(Severity::Info, "");
                                                     subdiagnostic.annotate(
                                                         Annotation::primary(self.context.span(
                                                             &call_expression.arguments.args[0],
@@ -4848,13 +4842,8 @@ impl<'db> TypeInferenceBuilder<'db> {
                                                         "Argument does not have expected type `Never`",
                                                     );
 
-                                                    let mut subdiagnostic = SubDiagnostic::new(
-                                                        Severity::Info,
-                                                        format_args!(
-                                                            "`{}` is not an equivalent type to `Never`",
-                                                            actual_ty.display(self.db()),
-                                                        ),
-                                                    );
+                                                    let mut subdiagnostic =
+                                                        SubDiagnostic::new(Severity::Info, "");
                                                     subdiagnostic.annotate(
                                                         Annotation::primary(self.context.span(
                                                             &call_expression.arguments.args[0],
```

</details>

which would render like this?

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/0bd9c8c9-55bc-4e2a-8270-60e7a131bbe2)

</details>

---

_Comment by @MichaReiser on 2025-05-12 17:38_

To make sure I understand the screenshot: Does the first diagnostic show the old format? Now that I'm seeing it, it sort of feels odd that the sub diagnostic has no message. Maybe it is better to remove the annotation message?

Do you have @BurntSushi any recommendation here?

---

_Comment by @AlexWaygood on 2025-05-12 17:40_

> To make sure I understand the screenshot: Does the first diagnostic show the old format?

no, both diagnostics in the new screenshot are what would be shown with the changes to my PR that I proposed in that diff. One diagnostic is for `assert_type`, the other is for `assert_never`.

---

_Comment by @MichaReiser on 2025-05-12 17:44_

Hmm okay. I'm a bit confused because it still repeats the actual (and even expected) type for `assert_never` and it seems inconsistent now that one has a message and the other doesn't.

I think I'd go with:

* primary message: Argument does not have expected type `<type>`
* primary annotation: Entire assert expression
* sub message: <inferred type> is not an equivalent type to <expected>
* sub annotation: "argument"

But curious to hear from Andrew.

---

_Comment by @AlexWaygood on 2025-05-12 17:47_

> Hmm okay. I'm a bit confused because it still repeats the actual (and even expected) type for `assert_never` and it seems inconsistent now that one has a message and the other doesn't.

Ah shoot, yes, I think that was a stale screenshot! Sorry for the confusion. Here's a new one:

<details>

![image](https://github.com/user-attachments/assets/6e9fee0d-77ff-4e98-b493-205ffb7a9970)

</details>

---

_Comment by @AlexWaygood on 2025-05-12 18:17_

@MichaReiser I pushed some updates which I think (at least partially) might address your comments? WDYT? I've edited the new screenshots into the PR description

---

_@AlexWaygood reviewed on 2025-05-12 18:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1 on 2025-05-12 18:19_

(none of the changes in this file are substantive. Just some driveby simplifications.)

---

_Comment by @MichaReiser on 2025-05-12 18:40_

Thanks

---

_@MichaReiser approved on 2025-05-12 18:40_

Thanks 



---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/assert_never.md_-_`assert_never`_-_Basic_functionality.snap`:49 on 2025-05-12 18:47_

I think this output is not ideal. In particular, it repeats the entire code snippet here. I'd suggest just this:

```
error[type-assertion-failure]: Argument does not have expected type `Never`
 --> src/mdtest_snippet.py:7:5
  |
5 |     assert_never(never)  # fine
6 |
7 |     assert_never(0)  # error: [type-assertion-failure]
  |                  ^  Inferred type is `Literal[0]`
8 |     assert_never("")  # error: [type-assertion-failure]
9 |     assert_never(None)  # error: [type-assertion-failure]
  |
```

I think this has better information density, has less duplication and contains all relevant context.

---

_@BurntSushi reviewed on 2025-05-12 18:50_

I left a suggestion that I think might help here.

As for the concise message format, I think it will prove very challenging to use one set of messages that balances both the concise and the verbose formats. They have fundamentally different goals and present information very different, and I worry that by trying to balance the two, we will wind up in a situation where both outputs are bad. Instead, I'd rather accept that the concise output is less helpful than it could be.

---

_@AlexWaygood reviewed on 2025-05-12 18:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assert_never.md_-_`assert_never`_-_Basic_functionality.snap`:49 on 2025-05-12 18:58_

Yes, I agree that there's some unfortunate duplication here. I'd love to only have a single annotation that only highlights the arguments passed in rather than the whole call.

Did you see the concern I mentioned in my PR summary about how diagnostic ranges interact with suppression comments? That's why I held off from doing that in this PR. If the range of the diagnostic is only the range of the arguments passed into the call, rather than the whole call, it's going to mean that comments like this will not suppress the diagnostic:

```py
assert_type(  # type: ignore
    very_very_very_long_variable_name,
    very_very_very_long_type,
)
```

I think it's especially a concern for `assert_type`, which requires two arguments rather than one (so the call is more likely to be multiline).

---

_@BurntSushi reviewed on 2025-05-12 19:55_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/assert_never.md_-_`assert_never`_-_Basic_functionality.snap`:49 on 2025-05-12 19:55_

Oh I see, I did miss that. In particular:

> I feel like in general we might want to separate the concepts of "range the diagnostic has for suppression purposes" and "primary range that is highlighted when the diagnostic is rendered on the terminal"? But that's out of scope for this PR.

I would tentatively agree with that... But there's definitely downsides with that because it makes creating a diagnostic somewhat confusing.

What I'd suggest instead is to have two annotations on the top-level diagnostic. The primary annotation can be the range you want for suppression comments to work well. Then a secondary annotation for the specific argument that is invalid. IDK exactly how it would render, but something like this:

```
error[type-assertion-failure]: Argument does not have expected type `Never`
 --> src/mdtest_snippet.py:7:5
  |
5 |     assert_never(never)  # fine
6 |
7 |     assert_never(0)  # error: [type-assertion-failure]
  |     ^^^^^^^^^^^^^^^
  |                  |
  |                  |
  |                  ------^  Inferred type is `Literal[0]`
8 |     assert_never("")  # error: [type-assertion-failure]
9 |     assert_never(None)  # error: [type-assertion-failure]
  |
```

In this case, I left the message on the primary annotation blank. I'm not sure if that's the right call or not, but it seems reasonable.

---

_@AlexWaygood reviewed on 2025-05-13 00:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assert_never.md_-_`assert_never`_-_Basic_functionality.snap`:49 on 2025-05-13 00:58_

> What I'd suggest instead is to have two annotations on the top-level diagnostic.

I've pushed this change. The rendering looks a bit odd to me, in particular the way `_` underlines are surrounded by `^` underlines:

![image](https://github.com/user-attachments/assets/8e02a4ef-4699-4745-a204-5b2db36848e5)

but it's probably worth it to keep the diagnostics concise, as you say.

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-05-13 00:58_

---

_@BurntSushi approved on 2025-05-13 12:13_

Yeah I agree the rendering is not ideal here. But I think it's probably the least bad option available to us at the moment.

---

_Merged by @AlexWaygood on 2025-05-13 13:00_

---

_Closed by @AlexWaygood on 2025-05-13 13:00_

---

_Branch deleted on 2025-05-13 13:00_

---
