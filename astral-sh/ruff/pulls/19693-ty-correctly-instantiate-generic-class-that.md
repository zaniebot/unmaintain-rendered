```yaml
number: 19693
title: "[ty] Correctly instantiate generic class that inherits `__init__` from generic base class"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/inherit-context
created_at: 2025-08-01T16:49:58Z
updated_at: 2025-08-01T19:29:20Z
url: https://github.com/astral-sh/ruff/pull/19693
synced_at: 2026-01-12T15:56:45Z
```

# [ty] Correctly instantiate generic class that inherits `__init__` from generic base class

---

_@dcreager_

This is subtle, and the root cause became more apparent with #19604, since we now have many more cases of superclasses and subclasses using different typevars. The issue is easiest to see in the following:

```py
class C[T]:
    def __init__(self, t: T) -> None: ...

class D[U](C[T]):
    pass

reveal_type(C(1))  # revealed: C[int]
reveal_type(D(1))  # should be: D[int]
```

When instantiating a generic class, the `__init__` method inherits the generic context of that class. This lets our call binding machinery infer a specialization for that context.

Prior to this PR, the instantiation of `C` worked just fine. Its `__init__` method would inherit the `[T]` generic context, and we would infer `{T = int}` as the specialization based on the argument parameters.

It didn't work for `D`. The issue is that the `__init__` method was inheriting the generic context of the class where `__init__` was defined (here, `C` and `[T]`). At the call site, we would then infer `{T = int}` as the specialization — but that wouldn't help us specialize `D[U]`, since `D` does not have `T` in its generic context!

Instead, the `__init__` method should inherit the generic context of the class that we are performing the lookup on (here, `D` and `[U]`). That lets us correctly infer `{U = int}` as the specialization, which we can successfully apply to `D[U]`.

(Note that `__init__` refers to `C`'s typevars in its signature, but that's okay; our member lookup logic already applies the `T = U` specialization when returning a member of `C` while performing a lookup on `D`, transforming its signature from `(Self, T) -> None` to `(Self, U) -> None`.)

Closes https://github.com/astral-sh/ty/issues/588

---

_Review requested from @carljm by @dcreager on 2025-08-01 16:49_

---

_Review requested from @AlexWaygood by @dcreager on 2025-08-01 16:49_

---

_Review requested from @sharkdp by @dcreager on 2025-08-01 16:49_

---

_Label `ty` added by @dcreager on 2025-08-01 16:49_

---

_Comment by @github-actions[bot] on 2025-08-01 16:51_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-01 16:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyp (https://github.com/hauntsaninja/pyp)
- tests/test_pyp.py:45:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- Found 7 diagnostics
+ Found 6 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/pq/pq_ctypes.py:94:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T@py_object`, found `ReferenceType[Unknown]`
- Found 656 diagnostics
+ Found 655 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/sansio/request.py:305:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
- src/werkzeug/sansio/request.py:318:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `<class 'int'>`
- src/werkzeug/sansio/request.py:503:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_set_header(value: str | None, on_update: ((HeaderSet, /) -> None) | None = None) -> HeaderSet`
- src/werkzeug/sansio/response.py:336:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_age(value: str | None = None) -> timedelta | None`
- src/werkzeug/sansio/response.py:355:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `<class 'int'>`
- src/werkzeug/sansio/response.py:391:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
- src/werkzeug/sansio/response.py:392:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def http_date(timestamp: date | int | float | struct_time | None = None) -> str`
- src/werkzeug/sansio/response.py:404:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
- src/werkzeug/sansio/response.py:405:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def http_date(timestamp: date | int | float | struct_time | None = None) -> str`
- src/werkzeug/sansio/response.py:417:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
- src/werkzeug/sansio/response.py:418:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def http_date(timestamp: date | int | float | struct_time | None = None) -> str`
- src/werkzeug/sansio/response.py:715:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_set_header(value: str | None, on_update: ((HeaderSet, /) -> None) | None = None) -> HeaderSet`
- src/werkzeug/sansio/response.py:716:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def dump_header(iterable: Iterable[Any]) -> str`
- src/werkzeug/sansio/response.py:722:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_set_header(value: str | None, on_update: ((HeaderSet, /) -> None) | None = None) -> HeaderSet`
- src/werkzeug/sansio/response.py:723:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def dump_header(iterable: Iterable[Any]) -> str`
- src/werkzeug/sansio/response.py:734:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_set_header(value: str | None, on_update: ((HeaderSet, /) -> None) | None = None) -> HeaderSet`
- src/werkzeug/sansio/response.py:735:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def dump_header(iterable: Iterable[Any]) -> str`
- src/werkzeug/sansio/response.py:741:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `<class 'int'>`
- tests/test_utils.py:153:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_TAccessorValue@environ_property | None`, found `Literal["spam"]`
- tests/test_utils.py:155:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@environ_property) | None`, found `<class 'int'>`
- tests/test_utils.py:156:65: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@environ_property) | None`, found `<class 'int'>`
- tests/test_utils.py:158:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@environ_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
- tests/test_utils.py:158:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@environ_property, /) -> str) | None`, found `def http_date(timestamp: date | int | float | struct_time | None = None) -> str`
- Found 390 diagnostics
+ Found 367 diagnostics

comtypes (https://github.com/enthought/comtypes)
- comtypes/test/test_clear_cache.py:19:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- comtypes/test/test_client.py:81:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- comtypes/test/test_client.py:104:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- comtypes/test/test_client.py:305:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- comtypes/test/test_getactiveobj.py:8:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- comtypes/test/test_ienum.py:11:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- comtypes/test/test_imfattributes.py:11:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- comtypes/test/test_monikers.py:10:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- comtypes/test/test_storage.py:11:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- comtypes/test/test_viewobject.py:15:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- Found 417 diagnostics
+ Found 407 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_testing_raisesgroup.py:40:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:84:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[RuntimeError, ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:140:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ExceptionGroup[Exception]]`
- src/trio/_tests/test_testing_raisesgroup.py:140:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:343:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:384:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:435:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:996:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:1008:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:1045:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[OSError]`
- src/trio/_tests/test_testing_raisesgroup.py:1111:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:24:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:30:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:133:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:156:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ExceptionGroup[Exception]]`
- src/trio/_tests/type_tests/raisesgroup.py:156:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- Found 737 diagnostics
+ Found 721 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- tests/test_iostream.py:168:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/test_iostream.py:176:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/test_iostream.py:187:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stderr`, found `StringIO`
- tests/test_iostream.py:195:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stderr`, found `StringIO`
- tests/test_iostream.py:205:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/test_iostream.py:225:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/test_iostream.py:232:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/test_iostream.py:239:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/test_iostream.py:251:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stderr`, found `StringIO`
- tests/test_iostream.py:266:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/test_iostream.py:266:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stderr`, found `StringIO`
- Found 221 diagnostics
+ Found 210 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/porcupine_debug_prompt.py:62:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- porcupine/plugins/porcupine_debug_prompt.py:62:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stderr`, found `StringIO`
- Found 24 diagnostics
+ Found 22 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/directive.py:138:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((...) -> T@StrawberryDirectiveResolver) | staticmethod | classmethod`, found `(...) -> T@directive`
- Found 374 diagnostics
+ Found 373 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- aiohttp_devtools/logs.py:32:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Formatter[str]`, found `Terminal256Formatter[_T@Terminal256Formatter]`
- Found 58 diagnostics
+ Found 57 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/utils/isolated_build.py:198:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- Found 935 diagnostics
+ Found 934 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- tests/unittests/test_cli.py:276:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- Found 597 diagnostics
+ Found 596 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/livereload_tests.py:291:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stderr`, found `StringIO`
- Found 184 diagnostics
+ Found 183 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/test_yaml.py:284:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/test_yaml.py:299:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- Found 155 diagnostics
+ Found 153 diagnostics

vision (https://github.com/pytorch/vision)
- references/detection/coco_eval.py:34:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- references/detection/coco_eval.py:190:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- Found 1482 diagnostics
+ Found 1480 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/helpers.py:377:11: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Parser[Message[str, str]]`, found `HeaderParser[_MessageT@HeaderParser]`
- Found 173 diagnostics
+ Found 172 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/utils/debug.py:35:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `Unknown | TextIO`
- test/mitmproxy/utils/test_arg_check.py:54:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- web/gen/options_js.py:40:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- Found 1826 diagnostics
+ Found 1823 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ lib/streamlit/config.py:39:62: warning[unused-ignore-comment] Unused blanket `ty: ignore` directive
+ lib/streamlit/config.py:40:61: warning[unused-ignore-comment] Unused blanket `ty: ignore` directive
- Found 146 diagnostics
+ Found 148 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/test/unit/test_archive_npy.py:658:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stderr`, found `StringIO`
- Found 1774 diagnostics
+ Found 1773 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/mintro.py:538:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
- Found 777 diagnostics
+ Found 776 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/_internal/concurrency/cancellation.py:598:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T@py_object`, found `type[BaseException]`
- src/prefect/server/database/_migrations/versions/postgresql/2023_09_21_130125_4e9a6f93eb6c_make_slot_decay_per_second_not_nullable.py:27:23: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/prefect/server/database/_migrations/versions/postgresql/2023_09_21_130125_4e9a6f93eb6c_make_slot_decay_per_second_not_nullable.py:36:23: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/prefect/server/database/_migrations/versions/sqlite/2023_09_21_121806_8167af8df781_make_slot_decay_per_second_not_nullable.py:26:52: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/prefect/server/database/_migrations/versions/sqlite/2023_09_21_121806_8167af8df781_make_slot_decay_per_second_not_nullable.py:33:52: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 3849 diagnostics
+ Found 3844 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/actions/message_send.py:1836:5: error[invalid-assignment] Invalid assignment to data descriptor attribute `type` on type `Message` with custom `__set__` method
- zerver/lib/timeout.py:69:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T@py_object`, found `<class 'TimeoutExpiredError'>`
- zerver/management/commands/send_test_email.py:48:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stderr`, found `StringIO`
- zerver/migrations/0001_squashed_0569.py:919:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@SmallIntegerField`, found `Literal[1]`
- zerver/migrations/0001_squashed_0569.py:1068:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@PositiveSmallIntegerField`, found `Literal[1]`
- zerver/migrations/0001_squashed_0569.py:1146:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@PositiveSmallIntegerField`, found `Literal[1]`
- zerver/migrations/0001_squashed_0569.py:1649:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@SmallIntegerField`, found `Literal[1]`
- zerver/migrations/0501_delete_dangling_usermessages.py:156:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `Unknown | TextIO`
- zerver/migrations/0536_add_message_type.py:16:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@PositiveSmallIntegerField`, found `Literal[1]`
- zerver/migrations/0536_add_message_type.py:23:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@PositiveSmallIntegerField`, found `Literal[1]`
- zerver/migrations/0548_realmuserdefault_web_channel_default_view_and_more.py:15:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@SmallIntegerField`, found `Literal[1]`
- zerver/migrations/0548_realmuserdefault_web_channel_default_view_and_more.py:20:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@SmallIntegerField`, found `Literal[1]`
- zerver/migrations/0578_namedusergroup_deactivated.py:15:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[False]`
- zerver/migrations/0586_customprofilefield_editable_by_user.py:15:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- zerver/migrations/0631_stream_is_recently_active.py:15:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- zerver/migrations/0703_realmuserdefault_resolved_topic_notice_auto_read_policy_and_more.py:18:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@PositiveSmallIntegerField`, found `Literal[2]`
- zerver/migrations/0703_realmuserdefault_resolved_topic_notice_auto_read_policy_and_more.py:26:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@PositiveSmallIntegerField`, found `Literal[2]`
- zerver/migrations/0704_stream_subscriber_count.py:15:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@PositiveIntegerField`, found `Literal[0]`
- zerver/migrations/0725_realmuserdefault_web_left_sidebar_unreads_count_summary_and_more.py:15:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- zerver/migrations/0725_realmuserdefault_web_left_sidebar_unreads_count_summary_and_more.py:20:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- zerver/migrations/0743_realm_require_e2ee_push_notifications.py:31:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[False]`
- zerver/migrations/0745_usersetting_web_left_sidebar_show_channel_folders.py:15:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- zerver/migrations/0745_usersetting_web_left_sidebar_show_channel_folders.py:20:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- zerver/models/custom_profile_fields.py:86:58: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- zerver/models/groups.py:87:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[False]`
- zerver/models/messages.py:58:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@PositiveSmallIntegerField`, found `Literal[MessageType.NORMAL]`
- zerver/models/realms.py:194:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[False]`
- zerver/models/streams.py:48:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@PositiveIntegerField`, found `Literal[0]`
- zerver/models/streams.py:179:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- zerver/models/users.py:117:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- zerver/models/users.py:152:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@SmallIntegerField`, found `Literal[1]`
- zerver/models/users.py:192:80: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[NOT_PROVIDED] | Expression | _ST@BooleanField`, found `Literal[True]`
- Found 7455 diagnostics
+ Found 7425 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- test/limits/mzcompose.py:2074:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- test/limits/mzcompose.py:2160:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- Found 3260 diagnostics
+ Found 3258 diagnostics

manticore (https://github.com/trailofbits/manticore)
- tests/ethereum/test_general.py:81:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/native/test_state.py:318:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/native/test_state.py:342:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/native/test_unicorn_concrete.py:160:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- tests/other/test_state_introspection.py:60:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- Found 1100 diagnostics
+ Found 1095 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-08-01 17:08_

Nice! I think you also need to pass the generic context into the `__new__` method we synthesize for tuple types here:

https://github.com/astral-sh/ruff/blob/06cd249a9be67b8cadc60a1557c2faaf56d7bac7/crates/ty_python_semantic/src/types/class.rs#L783-L827

Example failing test case on your branch:

```py
class Foo[T, U](tuple[T, U]): ...  # false positive error: Argument is incorrect: Expected `tuple[T@Foo, U@Foo]`, found `tuple[Literal[1], Literal[2]]` (invalid-argument-type)
reveal_type(Foo((1, 2)))  # revealed: Foo[Unknown, Unknown]
```

---

_Comment by @AlexWaygood on 2025-08-01 17:09_

Love that primer report!!

---

_Comment by @dcreager on 2025-08-01 18:20_

> I think you also need to pass the generic context into the `__new__` method we synthesize for tuple types here

Thank you! Done

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:380 on 2025-08-01 18:31_

I sort of wish that this was `C[Literal[1], Literal[2]]` -- I don't think there's a strong reason to upcast the `Literal` types to `int` types here, since `tuple` is covariant. We'd obviously infer `tuple[Literal[1], Literal[2]]` rather than `tuple[int, int]` for `(1, 2)`, for similar reasons: the covariance of `tuple` means that `tuple[Literal[1], Literal[2]]` is assignable to `tuple[int, int]`, so there's no way that inferring the more precise type can produce false positives.

But that's not for this PR! It's obviously a pre-existing issue.

---

_@AlexWaygood approved on 2025-08-01 18:31_

---

_@dcreager reviewed on 2025-08-01 19:29_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:380 on 2025-08-01 19:29_

Yep totally!

---

_Merged by @dcreager on 2025-08-01 19:29_

---

_Closed by @dcreager on 2025-08-01 19:29_

---

_Branch deleted on 2025-08-01 19:29_

---
