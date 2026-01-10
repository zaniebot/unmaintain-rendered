```yaml
number: 22019
title: "[ty] Improve disambiguation of types in many cases"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/qualified-display
created_at: 2025-12-17T10:27:11Z
updated_at: 2025-12-17T11:41:08Z
url: https://github.com/astral-sh/ruff/pull/22019
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Improve disambiguation of types in many cases

---

_Pull request opened by @AlexWaygood on 2025-12-17 10:27_

## Summary

### The "display disambiguator"

For many diagnostics, we run the "display disambiguator" on all types that will appear in that diagnostic. This is to avoid confusing diagnostic messages in situations where two files have types that are distinct but share the same name -- e.g. without the "display disambiguator", we'd end up with messages like this:

```
error [invalid-return-type] Expected type `Foo`, got type `Foo`
```

whereas with it, we get

```
error [invalid-return-type] Expected type `module.Foo`, got type `main.Foo`
```

(The "display disambiguator" means that we only use the fully qualified name for _actually ambiguous_ cases. Types that are not ambiguous still use the unqualified names in diagnostic messages.)

### This PR

This PR extends our usage of the display disambiguator in two ways:
- We now run it by default in all calls to `Type::display()`. Even if there is only one type that needs to be rendered in a diagnostic message, that type might _contain_ lots of other types and therefore require disambiguation. A common example that we've seen in the ecosystem is `numpy.bool[builtins.bool]` being rendered confusingly as `bool[bool]`.
- I added calls to the display disambiguator to our `invalid-argument-type` diagnostic -- this is a specific diagnostic that involves a pair of types, where we weren't running it previously.

## Test Plan

Mdtests/snapshots added and updated.


---

_Label `ty` added by @AlexWaygood on 2025-12-17 10:27_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-17 10:27_

---

_Label `ty` added by @AlexWaygood on 2025-12-17 10:27_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-17 10:27_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 10:29_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 10:31_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:883:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `CancelScope | None`, found `CancelScope`
+ src/anyio/_backends/_asyncio.py:883:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `anyio._backends._asyncio.CancelScope | None`, found `anyio._core._tasks.CancelScope`

spack (https://github.com/spack/spack)
- lib/spack/spack/database.py:949:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Lock`, found `ForbiddenLock | Lock`
+ lib/spack/spack/database.py:949:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `lib.spack.spack.llnl.util.lock.Lock`, found `ForbiddenLock | lib.spack.spack.util.lock.Lock`

pip (https://github.com/pypa/pip)
- src/pip/_vendor/distlib/util.py:1283:9: error[invalid-assignment] Object of type `def extraction_filter(member, path) -> Unknown` is not assignable to attribute `extraction_filter` on type `ZipFile | ZipFile | TarFile`
+ src/pip/_vendor/distlib/util.py:1283:9: error[invalid-assignment] Object of type `def extraction_filter(member, path) -> Unknown` is not assignable to attribute `extraction_filter` on type `zipfile.ZipFile | pip._vendor.distlib.compat.ZipFile | TarFile`
- src/pip/_vendor/distlib/util.py:1675:20: warning[unsupported-base] Unsupported class base with type `<class 'BaseConfigurator'> | <class 'BaseConfigurator'>`
+ src/pip/_vendor/distlib/util.py:1675:20: warning[unsupported-base] Unsupported class base with type `<class 'logging.config.BaseConfigurator'> | <class 'pip._vendor.distlib.compat.BaseConfigurator'>`
- src/pip/_vendor/msgpack/fallback.py:929:20: warning[possibly-missing-attribute] Attribute `getbuffer` may be missing on object of type `Unknown | BytesIO | BytesIO`
+ src/pip/_vendor/msgpack/fallback.py:929:20: warning[possibly-missing-attribute] Attribute `getbuffer` may be missing on object of type `Unknown | pip._vendor.msgpack.fallback.BytesIO | _io.BytesIO`
- src/pip/_vendor/urllib3/contrib/pyopenssl.py:135:5: error[invalid-assignment] Object of type `<class 'PyOpenSSLContext'>` is not assignable to attribute `SSLContext` of type `<class 'SSLContext'> | <class 'SSLContext'>`
+ src/pip/_vendor/urllib3/contrib/pyopenssl.py:135:5: error[invalid-assignment] Object of type `<class 'PyOpenSSLContext'>` is not assignable to attribute `SSLContext` of type `<class 'ssl.SSLContext'> | <class 'pip._vendor.urllib3.util.ssl_.SSLContext'>`
- src/pip/_vendor/urllib3/contrib/pyopenssl.py:136:5: error[invalid-assignment] Object of type `<class 'PyOpenSSLContext'>` is not assignable to attribute `SSLContext` of type `<class 'SSLContext'> | <class 'SSLContext'>`
+ src/pip/_vendor/urllib3/contrib/pyopenssl.py:136:5: error[invalid-assignment] Object of type `<class 'PyOpenSSLContext'>` is not assignable to attribute `SSLContext` of type `<class 'ssl.SSLContext'> | <class 'pip._vendor.urllib3.util.ssl_.SSLContext'>`
- src/pip/_vendor/urllib3/contrib/securetransport.py:192:5: error[invalid-assignment] Object of type `<class 'SecureTransportContext'>` is not assignable to attribute `SSLContext` of type `<class 'SSLContext'> | <class 'SSLContext'>`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:192:5: error[invalid-assignment] Object of type `<class 'SecureTransportContext'>` is not assignable to attribute `SSLContext` of type `<class 'ssl.SSLContext'> | <class 'pip._vendor.urllib3.util.ssl_.SSLContext'>`
- src/pip/_vendor/urllib3/contrib/securetransport.py:193:5: error[invalid-assignment] Object of type `<class 'SecureTransportContext'>` is not assignable to attribute `SSLContext` of type `<class 'SSLContext'> | <class 'SSLContext'>`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:193:5: error[invalid-assignment] Object of type `<class 'SecureTransportContext'>` is not assignable to attribute `SSLContext` of type `<class 'ssl.SSLContext'> | <class 'pip._vendor.urllib3.util.ssl_.SSLContext'>`
- src/pip/_vendor/urllib3/util/ssl_.py:332:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `post_handshake_auth` on type `Unknown | SSLContext | SSLContext`
+ src/pip/_vendor/urllib3/util/ssl_.py:332:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `post_handshake_auth` on type `Unknown | ssl.SSLContext | pip._vendor.urllib3.util.ssl_.SSLContext`
- src/pip/_vendor/urllib3/util/ssl_.py:359:13: error[invalid-assignment] Object of type `str & ~AlwaysFalsy` is not assignable to attribute `keylog_filename` on type `(Unknown & <Protocol with members 'keylog_filename'>) | SSLContext | (SSLContext & <Protocol with members 'keylog_filename'>)`
+ src/pip/_vendor/urllib3/util/ssl_.py:359:13: error[invalid-assignment] Object of type `str & ~AlwaysFalsy` is not assignable to attribute `keylog_filename` on type `(Unknown & <Protocol with members 'keylog_filename'>) | ssl.SSLContext | (pip._vendor.urllib3.util.ssl_.SSLContext & <Protocol with members 'keylog_filename'>)`

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/serving.py:879:25: warning[unsupported-base] Unsupported class base with type `<class 'ForkingMixIn'> | <class 'ForkingMixIn'>`
+ src/werkzeug/serving.py:879:25: warning[unsupported-base] Unsupported class base with type `<class 'socketserver.ForkingMixIn'> | <class 'werkzeug.serving.ForkingMixIn'>`

pybind11 (https://github.com/pybind/pybind11)
- pybind11/setup_helpers.py:89:25: warning[unsupported-base] Unsupported class base with type `<class 'Extension'> | <class 'Extension'>`
+ pybind11/setup_helpers.py:89:25: warning[unsupported-base] Unsupported class base with type `<class 'setuptools.extension.Extension'> | <class 'distutils.extension.Extension'>`
- pybind11/setup_helpers.py:271:17: warning[unsupported-base] Unsupported class base with type `<class 'build_ext'> | <class 'build_ext'>`
+ pybind11/setup_helpers.py:271:17: warning[unsupported-base] Unsupported class base with type `<class 'setuptools.command.build_ext.build_ext'> | <class 'distutils.command.build_ext.build_ext'>`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/web.py:733:51: error[invalid-argument-type] Argument is incorrect: Expected `StartResponse`, found `StartResponse | StartResponse`
+ dulwich/web.py:733:51: error[invalid-argument-type] Argument is incorrect: Expected `_typeshed.wsgi.StartResponse`, found `_typeshed.wsgi.StartResponse | dulwich.web.StartResponse`
- dulwich/web.py:762:34: error[invalid-argument-type] Argument is incorrect: Expected `StartResponse`, found `StartResponse | StartResponse`
+ dulwich/web.py:762:34: error[invalid-argument-type] Argument is incorrect: Expected `_typeshed.wsgi.StartResponse`, found `_typeshed.wsgi.StartResponse | dulwich.web.StartResponse`
- dulwich/web.py:787:34: error[invalid-argument-type] Argument is incorrect: Expected `StartResponse`, found `StartResponse | StartResponse`
+ dulwich/web.py:787:34: error[invalid-argument-type] Argument is incorrect: Expected `_typeshed.wsgi.StartResponse`, found `_typeshed.wsgi.StartResponse | dulwich.web.StartResponse`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:808:25: error[invalid-argument-type] Argument to function `future_set_result_unless_cancelled` is incorrect: Expected `Future[Any] | Future[Any]`, found `Unknown | Future[_T@__init__] | None`
+ tornado/gen.py:808:25: error[invalid-argument-type] Argument to function `future_set_result_unless_cancelled` is incorrect: Expected `concurrent.futures._base.Future[Any] | _asyncio.Future[Any]`, found `Unknown | _asyncio.Future[_T@__init__] | None`
- tornado/gen.py:815:41: error[invalid-argument-type] Argument to function `future_set_exc_info` is incorrect: Expected `Future[Unknown] | Future[Unknown]`, found `Unknown | Future[_T@__init__] | None`
+ tornado/gen.py:815:41: error[invalid-argument-type] Argument to function `future_set_exc_info` is incorrect: Expected `concurrent.futures._base.Future[Unknown] | _asyncio.Future[Unknown]`, found `Unknown | _asyncio.Future[_T@__init__] | None`
- tornado/iostream.py:1295:48: error[invalid-argument-type] Argument to function `future_set_result_unless_cancelled` is incorrect: Expected `Future[Self@_handle_connect] | Future[Self@_handle_connect]`, found `(Unknown & ~None) | Future[IOStream]`
+ tornado/iostream.py:1295:48: error[invalid-argument-type] Argument to function `future_set_result_unless_cancelled` is incorrect: Expected `concurrent.futures._base.Future[Self@_handle_connect] | _asyncio.Future[Self@_handle_connect]`, found `(Unknown & ~None) | _asyncio.Future[IOStream]`
- tornado/web.py:2465:48: error[invalid-argument-type] Argument to function `future_set_result_unless_cancelled` is incorrect: Expected `Future[None] | Future[None]`, found `Unknown | None`
+ tornado/web.py:2465:48: error[invalid-argument-type] Argument to function `future_set_result_unless_cancelled` is incorrect: Expected `concurrent.futures._base.Future[None] | _asyncio.Future[None]`, found `Unknown | None`

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/pandas/array.py:76:13: error[invalid-argument-type] Argument to function `series_strategy` is incorrect: Expected `DataType | DataType`, found `DataType`
+ pandera/api/pandas/array.py:76:13: error[invalid-argument-type] Argument to function `series_strategy` is incorrect: Expected `pandera.engines.numpy_engine.DataType | pandera.engines.pandas_engine.DataType`, found `pandera.dtypes.DataType`
- pandera/api/pandas/components.py:190:13: error[invalid-argument-type] Argument to function `column_strategy` is incorrect: Expected `DataType | DataType`, found `DataType`
+ pandera/api/pandas/components.py:190:13: error[invalid-argument-type] Argument to function `column_strategy` is incorrect: Expected `pandera.engines.numpy_engine.DataType | pandera.engines.pandas_engine.DataType`, found `pandera.dtypes.DataType`
- pandera/api/pandas/components.py:263:13: error[invalid-argument-type] Argument to function `column_strategy` is incorrect: Expected `DataType | DataType`, found `DataType`
+ pandera/api/pandas/components.py:263:13: error[invalid-argument-type] Argument to function `column_strategy` is incorrect: Expected `pandera.engines.numpy_engine.DataType | pandera.engines.pandas_engine.DataType`, found `pandera.dtypes.DataType`
- pandera/engines/numpy_engine.py:271:15: warning[unsupported-base] Unsupported class base with type `<class 'Float64'> | <class 'Float64'>`
+ pandera/engines/numpy_engine.py:271:15: warning[unsupported-base] Unsupported class base with type `<class 'pandera.engines.numpy_engine.Float64 @ pandera/engines/numpy_engine.py:256'> | <class 'pandera.engines.numpy_engine.Float64 @ pandera/engines/numpy_engine.py:264'>`
- pandera/engines/numpy_engine.py:320:17: warning[unsupported-base] Unsupported class base with type `<class 'Complex128'> | <class 'Complex128'>`
+ pandera/engines/numpy_engine.py:320:17: warning[unsupported-base] Unsupported class base with type `<class 'pandera.engines.numpy_engine.Complex128 @ pandera/engines/numpy_engine.py:305'> | <class 'pandera.engines.numpy_engine.Complex128 @ pandera/engines/numpy_engine.py:313'>`
- pandera/engines/pandas_engine.py:1390:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: int | signedinteger[_64Bit] | integer[Any] | signedinteger[_8Bit]) -> Unknown, (idx: Index[Any] | Series[Any] | slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]]) -> Series[Unknown], (idx: str | bytes | date | ... omitted 10 union elements) -> Unknown, (idx: Series[bool] | ndarray[tuple[Any, ...], dtype[bool[bool]]] | list[bool] | ... omitted 7 union elements) -> Series[Unknown]]` cannot be called with key of type `Series[bool] | DataFrame` on object of type `Series[Unknown]`
+ pandera/engines/pandas_engine.py:1390:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: int | signedinteger[_64Bit] | integer[Any] | signedinteger[_8Bit]) -> Unknown, (idx: Index[Any] | Series[Any] | slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]]) -> Series[Unknown], (idx: str | bytes | date | ... omitted 10 union elements) -> Unknown, (idx: Series[builtins.bool] | ndarray[tuple[Any, ...], dtype[numpy.bool[builtins.bool]]] | list[builtins.bool] | ... omitted 7 union elements) -> Series[Unknown]]` cannot be called with key of type `Series[bool] | DataFrame` on object of type `Series[Unknown]`
- tests/mypy/pandas_modules/pandas_dataframe.py:54:4: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `DataFrame[Schema]`, found `DataFrame`
+ tests/mypy/pandas_modules/pandas_dataframe.py:54:4: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `pandera.typing.pandas.DataFrame[Schema]`, found `pandas.core.frame.DataFrame`
- tests/mypy/pandas_modules/pandera_types.py:14:4: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `Series[int]`, found `Series[Any]`
+ tests/mypy/pandas_modules/pandera_types.py:14:4: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `pandera.typing.pandas.Series[int]`, found `pandas.core.series.Series[Any]`
- tests/mypy/pandas_modules/pandera_types.py:15:4: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `Series[int]`, found `Series[Any]`
+ tests/mypy/pandas_modules/pandera_types.py:15:4: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `pandera.typing.pandas.Series[int]`, found `pandas.core.series.Series[Any]`
- tests/pandas/test_decorators.py:863:32: error[invalid-argument-type] Argument to function `transform_with_literal` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:863:32: error[invalid-argument-type] Argument to function `transform_with_literal` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:865:36: error[invalid-argument-type] Argument to function `transform_with_literal` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:865:36: error[invalid-argument-type] Argument to function `transform_with_literal` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1058:41: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1058:41: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1058:47: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1058:47: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1059:41: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1059:41: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1059:47: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1059:47: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1059:53: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1059:53: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1064:38: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1064:38: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1064:44: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1064:44: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1064:50: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1064:50: error[invalid-argument-type] Argument to function `get_len_star_args__dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1093:9: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `DataFrame[InSchema] | None`, found `DataFrame`
+ tests/pandas/test_decorators.py:1093:9: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema] | None`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1094:9: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1094:9: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1097:9: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `DataFrame[InSchema] | None`, found `DataFrame`
+ tests/pandas/test_decorators.py:1097:9: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema] | None`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1097:22: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1097:22: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1097:35: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1097:35: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1108:13: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `DataFrame[InSchema] | None`, found `DataFrame`
+ tests/pandas/test_decorators.py:1108:13: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema] | None`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1108:26: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1108:26: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1108:39: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1108:39: error[invalid-argument-type] Argument to function `get_star_kwargs_keys_dataframe` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1134:9: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1134:9: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1134:15: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1134:15: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1134:21: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1134:21: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1134:27: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1134:27: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1134:40: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1134:40: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_decorators.py:1134:53: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_decorators.py:1134:53: error[invalid-argument-type] Argument to function `star_args_kwargs` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_from_to_format_conversions.py:289:16: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_from_to_format_conversions.py:289:16: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_from_to_format_conversions.py:293:18: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_from_to_format_conversions.py:293:18: error[invalid-argument-type] Argument to function `fn` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_from_to_format_conversions.py:306:20: error[invalid-argument-type] Argument to function `invalid_fn` is incorrect: Expected `DataFrame[InSchema]`, found `DataFrame`
+ tests/pandas/test_from_to_format_conversions.py:306:20: error[invalid-argument-type] Argument to function `invalid_fn` is incorrect: Expected `pandera.typing.pandas.DataFrame[InSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_logical_dtypes.py:269:9: error[invalid-argument-type] Argument to bound method `check` is incorrect: Expected `DataType`, found `DataType`
+ tests/pandas/test_logical_dtypes.py:269:9: error[invalid-argument-type] Argument to bound method `check` is incorrect: Expected `pandera.engines.pandas_engine.DataType`, found `pandera.dtypes.DataType`
- tests/pandas/test_pydantic.py:74:39: error[invalid-argument-type] Argument is incorrect: Expected `DataFrame[SimpleSchema]`, found `DataFrame`
+ tests/pandas/test_pydantic.py:74:39: error[invalid-argument-type] Argument is incorrect: Expected `pandera.typing.pandas.DataFrame[SimpleSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_pydantic.py:78:25: error[invalid-argument-type] Argument is incorrect: Expected `DataFrame[SimpleSchema]`, found `DataFrame`
+ tests/pandas/test_pydantic.py:78:25: error[invalid-argument-type] Argument is incorrect: Expected `pandera.typing.pandas.DataFrame[SimpleSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_pydantic.py:250:42: error[invalid-argument-type] Argument is incorrect: Expected `DataFrame[SimpleSchema]`, found `DataFrame`
+ tests/pandas/test_pydantic.py:250:42: error[invalid-argument-type] Argument is incorrect: Expected `pandera.typing.pandas.DataFrame[SimpleSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_pydantic.py:254:46: error[invalid-argument-type] Argument is incorrect: Expected `DataFrame[SimpleSchema]`, found `DataFrame`
+ tests/pandas/test_pydantic.py:254:46: error[invalid-argument-type] Argument is incorrect: Expected `pandera.typing.pandas.DataFrame[SimpleSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_pydantic_dtype.py:60:22: error[invalid-argument-type] Argument to function `func` is incorrect: Expected `DataFrame[PydanticSchema]`, found `DataFrame`
+ tests/pandas/test_pydantic_dtype.py:60:22: error[invalid-argument-type] Argument to function `func` is incorrect: Expected `pandera.typing.pandas.DataFrame[PydanticSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_pydantic_dtype.py:68:14: error[invalid-argument-type] Argument to function `func` is incorrect: Expected `DataFrame[PydanticSchema]`, found `DataFrame`
+ tests/pandas/test_pydantic_dtype.py:68:14: error[invalid-argument-type] Argument to function `func` is incorrect: Expected `pandera.typing.pandas.DataFrame[PydanticSchema]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_schema_forwardref.py:25:10: error[invalid-argument-type] Argument to function `func` is incorrect: Expected `DataFrame[Model]`, found `DataFrame`
+ tests/pandas/test_schema_forwardref.py:25:10: error[invalid-argument-type] Argument to function `func` is incorrect: Expected `pandera.typing.pandas.DataFrame[Model]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_schema_forwardref.py:27:14: error[invalid-argument-type] Argument to function `func` is incorrect: Expected `DataFrame[Model]`, found `DataFrame`
+ tests/pandas/test_schema_forwardref.py:27:14: error[invalid-argument-type] Argument to function `func` is incorrect: Expected `pandera.typing.pandas.DataFrame[Model]`, found `pandas.core.frame.DataFrame`
- tests/pandas/test_schema_forwardref.py:46:20: error[invalid-argument-type] Argument to function `local_func` is incorrect: Expected `DataFrame[LocalModel]`, found `DataFrame`
+ tests/pandas/test_schema_forwardref.py:46:20: error[invalid-argument-type] Argument to function `local_func` is incorrect: Expected `pandera.typing.pandas.DataFrame[LocalModel]`, found `pandas.core.frame.DataFrame`
- tests/polars/test_polars_pydantic.py:189:46: error[invalid-argument-type] Argument is incorrect: Expected `DataFrame[SimplePolarsSchema]`, found `DataFrame`
+ tests/polars/test_polars_pydantic.py:189:46: error[invalid-argument-type] Argument is incorrect: Expected `pandera.typing.polars.DataFrame[SimplePolarsSchema]`, found `pandas.core.frame.DataFrame`
- tests/strategies/test_strategies.py:389:13: error[invalid-argument-type] Argument to function `str_length_strategy` is incorrect: Expected `DataType | DataType`, found `<class 'String'>`
+ tests/strategies/test_strategies.py:389:13: error[invalid-argument-type] Argument to function `str_length_strategy` is incorrect: Expected `pandera.engines.numpy_engine.DataType | pandera.engines.pandas_engine.DataType`, found `<class 'String'>`
- tests/strategies/test_strategies.py:394:9: error[invalid-argument-type] Argument to function `str_length_strategy` is incorrect: Expected `DataType | DataType`, found `<class 'String'>`
+ tests/strategies/test_strategies.py:394:9: error[invalid-argument-type] Argument to function `str_length_strategy` is incorrect: Expected `pandera.engines.numpy_engine.DataType | pandera.engines.pandas_engine.DataType`, found `<class 'String'>`

optuna (https://github.com/optuna/optuna)
- tests/storages_tests/test_with_server.py:86:30: error[not-iterable] Object of type `bool[bool]` is not iterable
+ tests/storages_tests/test_with_server.py:86:30: error[not-iterable] Object of type `numpy.bool[builtins.bool]` is not iterable
- tests/storages_tests/test_with_server.py:103:30: error[not-iterable] Object of type `bool[bool]` is not iterable
+ tests/storages_tests/test_with_server.py:103:30: error[not-iterable] Object of type `numpy.bool[builtins.bool]` is not iterable

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/utils.py:74:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:74:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`
- tests/utils.py:75:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:75:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`
- tests/utils.py:76:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:76:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`
- tests/utils.py:77:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:77:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`
- tests/utils.py:78:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:78:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`
- tests/utils.py:80:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:80:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`
- tests/utils.py:81:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:81:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`
- tests/utils.py:82:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:82:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`
- tests/utils.py:83:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:83:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`
- tests/utils.py:84:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `_LoggingWatcher`, found `_LoggingWatcher`
+ tests/utils.py:84:47: error[invalid-argument-type] Argument to function `filter_logs_by_level` is incorrect: Expected `tests.utils._LoggingWatcher`, found `unittest._log._LoggingWatcher`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/freqai/data_drawer.py:363:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:363:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqai/data_drawer.py:368:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:368:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqai/data_drawer.py:369:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:369:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqai/data_drawer.py:373:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:373:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqai/data_drawer.py:376:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:376:13: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqai/data_drawer.py:383:17: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `@Todo` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:383:17: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `@Todo` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqai/data_drawer.py:387:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:387:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqai/data_drawer.py:390:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:390:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqai/data_drawer.py:393:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:393:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqai/data_drawer.py:396:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:396:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `str | bytes | date | ... omitted 10 union elements` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/optimize/analysis/lookahead.py:84:30: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: int | signedinteger[_64Bit] | integer[Any] | signedinteger[_8Bit]) -> Any, (key: Index[Any] | Series[Any] | slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]]) -> Series[Any]]` cannot be called with key of type `int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]` on object of type `_iLocIndexerSeries[Any]`
+ freqtrade/optimize/analysis/lookahead.py:84:30: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: int | signedinteger[_64Bit] | integer[Any] | signedinteger[_8Bit]) -> Any, (key: Index[Any] | Series[Any] | slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]]) -> Series[Any]]` cannot be called with key of type `int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]` on object of type `_iLocIndexerSeries[Any]`

apprise (https://github.com/caronc/apprise)
- apprise/plugins/fcm/__init__.py:338:16: warning[possibly-missing-attribute] Attribute `load` may be missing on object of type `Unknown | GoogleOAuth | GoogleOAuth`
+ apprise/plugins/fcm/__init__.py:338:16: warning[possibly-missing-attribute] Attribute `load` may be missing on object of type `Unknown | apprise.plugins.fcm.oauth.GoogleOAuth | apprise.plugins.fcm.GoogleOAuth`
- apprise/plugins/fcm/__init__.py:345:28: warning[possibly-missing-attribute] Attribute `project_id` may be missing on object of type `Unknown | GoogleOAuth | GoogleOAuth`
+ apprise/plugins/fcm/__init__.py:345:28: warning[possibly-missing-attribute] Attribute `project_id` may be missing on object of type `Unknown | apprise.plugins.fcm.oauth.GoogleOAuth | apprise.plugins.fcm.GoogleOAuth`
- apprise/plugins/fcm/__init__.py:354:16: warning[possibly-missing-attribute] Attribute `access_token` may be missing on object of type `Unknown | GoogleOAuth | GoogleOAuth`
+ apprise/plugins/fcm/__init__.py:354:16: warning[possibly-missing-attribute] Attribute `access_token` may be missing on object of type `Unknown | apprise.plugins.fcm.oauth.GoogleOAuth | apprise.plugins.fcm.GoogleOAuth`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/encryption.py:196:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SSLContext | None`, found `Unknown | SSLContext | SSLContext`
+ pymongo/asynchronous/encryption.py:196:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `pymongo.pyopenssl_context.SSLContext | None`, found `Unknown | pymongo.pyopenssl_context.SSLContext | ssl.SSLContext`
- pymongo/ssl_support.py:105:13: error[invalid-assignment] Object of type `bool` is not assignable to attribute `check_ocsp_endpoint` on type `SSLContext | (SSLContext & <Protocol with members 'check_ocsp_endpoint'>)`
+ pymongo/ssl_support.py:105:13: error[invalid-assignment] Object of type `bool` is not assignable to attribute `check_ocsp_endpoint` on type `pymongo.pyopenssl_context.SSLContext | (ssl.SSLContext & <Protocol with members 'check_ocsp_endpoint'>)`
- pymongo/ssl_support.py:124:13: error[invalid-assignment] Object of type `Any | Literal[0]` is not assignable to attribute `verify_flags` on type `SSLContext | SSLContext`
+ pymongo/ssl_support.py:124:13: error[invalid-assignment] Object of type `Any | Literal[0]` is not assignable to attribute `verify_flags` on type `pymongo.pyopenssl_context.SSLContext | ssl.SSLContext`
- pymongo/synchronous/encryption.py:195:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SSLContext | None`, found `Unknown | SSLContext | SSLContext`
+ pymongo/synchronous/encryption.py:195:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `pymongo.pyopenssl_context.SSLContext | None`, found `Unknown | pymongo.pyopenssl_context.SSLContext | ssl.SSLContext`

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/tests/test_interface.py:286:41: warning[unsupported-base] Unsupported class base with type `<class 'CleanUp'> | <class 'CleanUp'>`
+ src/zope/interface/tests/test_interface.py:286:41: warning[unsupported-base] Unsupported class base with type `<class 'zope.interface.tests.CleanUp'> | <class 'zope.testing.cleanup.CleanUp'>`

vision (https://github.com/pytorch/vision)
- test/test_transforms_v2.py:4224:20: warning[possibly-missing-attribute] Attribute `float` may be missing on object of type `Unknown | Image | Image | Video`
+ test/test_transforms_v2.py:4224:20: warning[possibly-missing-attribute] Attribute `float` may be missing on object of type `Unknown | PIL.Image.Image | torchvision.tv_tensors._image.Image | Video`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/dist.py:974:44: error[invalid-argument-type] Argument to bound method `get_command_obj` is incorrect: Expected `str`, found `(str & ~Command) | (Command & ~Command)`
+ setuptools/_distutils/dist.py:974:44: error[invalid-argument-type] Argument to bound method `get_command_obj` is incorrect: Expected `str`, found `(str & ~distutils.cmd.Command) | (setuptools._distutils.cmd.Command & ~distutils.cmd.Command)`
- setuptools/_distutils/dist.py:978:16: warning[possibly-missing-attribute] Attribute `finalized` may be missing on object of type `(str & Command) | (Command & Command) | Unknown`
+ setuptools/_distutils/dist.py:978:16: warning[possibly-missing-attribute] Attribute `finalized` may be missing on object of type `(str & distutils.cmd.Command) | (setuptools._distutils.cmd.Command & distutils.cmd.Command) | Unknown`
- setuptools/_distutils/dist.py:981:9: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `finalized` on type `(str & Command) | (Command & Command) | Unknown`
+ setuptools/_distutils/dist.py:981:9: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `finalized` on type `(str & distutils.cmd.Command) | (setuptools._distutils.cmd.Command & distutils.cmd.Command) | Unknown`
- setuptools/_distutils/dist.py:982:9: error[invalid-assignment] Invalid subscript assignment with key of type `str | (Command & ~Command)` and value of type `Literal[False]` on object of type `dict[str, bool]`
+ setuptools/_distutils/dist.py:982:9: error[invalid-assignment] Invalid subscript assignment with key of type `str | (setuptools._distutils.cmd.Command & ~distutils.cmd.Command)` and value of type `Literal[False]` on object of type `dict[str, bool]`
- setuptools/command/editable_wheel.py:353:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `WheelFile`, found `WheelFile`
+ setuptools/command/editable_wheel.py:353:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `setuptools._vendor.wheel.wheelfile.WheelFile`, found `wheel.wheelfile.WheelFile`
- setuptools/tests/test_bdist_wheel.py:224:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Distribution`, found `Distribution`
+ setuptools/tests/test_bdist_wheel.py:224:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `setuptools.dist.Distribution`, found `distutils.dist.Distribution`
- setuptools/tests/test_editable_install.py:255:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Distribution`, found `Distribution`
+ setuptools/tests/test_editable_install.py:255:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `setuptools.dist.Distribution`, found `distutils.dist.Distribution`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dask/tests/test_task_runners.py:193:17: error[invalid-argument-type] Argument is incorrect: Expected `StateType`, found `Literal[StateType.COMPLETED]`
+ src/integrations/prefect-dask/tests/test_task_runners.py:193:17: error[invalid-argument-type] Argument is incorrect: Expected `prefect.client.schemas.objects.StateType`, found `Literal[prefect.server.schemas.states.StateType.COMPLETED]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
- src/integrations/prefect-redis/tests/test_messaging.py:180:41: error[invalid-argument-type] Argument to function `drain_one` is incorrect: Expected `Consumer`, found `Consumer`
+ src/integrations/prefect-redis/tests/test_messaging.py:180:41: error[invalid-argument-type] Argument to function `drain_one` is incorrect: Expected `prefect_redis.messaging.Consumer`, found `prefect.server.utilities.messaging.Consumer`
- src/integrations/prefect-redis/tests/test_messaging.py:326:45: error[invalid-argument-type] Argument to function `drain_one` is incorrect

... (truncated 801 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-17 10:52_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 11:00_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 0 | 250 |
| `type-assertion-failure` | 0 | 0 | 96 |
| `possibly-missing-attribute` | 0 | 0 | 64 |
| `invalid-assignment` | 0 | 0 | 37 |
| `unsupported-base` | 0 | 0 | 11 |
| `invalid-type-form` | 0 | 0 | 8 |
| `invalid-return-type` | 0 | 1 | 5 |
| `unresolved-attribute` | 0 | 0 | 5 |
| `invalid-super-argument` | 0 | 0 | 4 |
| `non-subscriptable` | 0 | 0 | 4 |
| `not-iterable` | 0 | 0 | 2 |
| **Total** | **0** | **1** | **486** |

**[Full report with detailed diff](https://alex-qualified-display.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-qualified-display.ecosystem-663.pages.dev/timing))




---

_Marked ready for review by @AlexWaygood on 2025-12-17 11:06_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-17 11:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-17 11:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-17 11:06_

---

_@MichaReiser approved on 2025-12-17 11:11_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-17 11:22_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-17 11:22_

---

_Comment by @AlexWaygood on 2025-12-17 11:36_

As followups, we need to improve our rendering of `invalid-super-argument` diagnostics and `unsupported-base` diagnostics. They get very verbose if it's a union of class-literal types, which doesn't seem uncommon.

---

_Comment by @AlexWaygood on 2025-12-17 11:36_

E.g.

```
error [unsupported-base] Unsupported class base with type `<class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:24'> | <class 'ibis.expr.types.rich.FixedTextJupyterMixin @ ibis/expr/types/rich.py:28'>`
```

and

```
error [invalid-super-argument] `<class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:17'> | <class 'ddtrace.contrib.internal.tornado.stack_context.TracerStackContext @ ddtrace/contrib/internal/tornado/stack_context.py:129'>` is not a valid class
```

---

_Merged by @AlexWaygood on 2025-12-17 11:41_

---

_Closed by @AlexWaygood on 2025-12-17 11:41_

---

_Branch deleted on 2025-12-17 11:41_

---
