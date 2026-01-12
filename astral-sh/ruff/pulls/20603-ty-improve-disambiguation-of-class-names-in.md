```yaml
number: 20603
title: "[ty] Improve disambiguation of class names in diagnostics"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/invalid-returns-generic-disambiguate
created_at: 2025-09-27T19:14:53Z
updated_at: 2025-09-29T10:43:13Z
url: https://github.com/astral-sh/ruff/pull/20603
synced_at: 2026-01-12T15:57:05Z
```

# [ty] Improve disambiguation of class names in diagnostics

---

_@AlexWaygood_

## Summary

Currently we'll use the fully qualified name of a class in _some_ cases when reporting instances where one class was expected but another one was found and the two classes have the same name. But there are still many cases where we won't do that -- e.g. in https://github.com/astral-sh/ruff/pull/20368, this is one of the new diagnostics:

```
error[invalid-return-type] Return type does not match returned value: expected `Iterable[Model]`, found `Iterable[Model]`
```

This PR switches the `DisplaySettings::from_possibly_ambiguous_type_pair()` function to use a `TypeVisitor` implementation, so that we can be confident it will deeply recurse into a pair of types and identify possibly ambiguous type names wherever they might be hiding.

## Test Plan

Mdtests


---

_Label `ty` added by @AlexWaygood on 2025-09-27 19:14_

---

_Renamed from "alex/invalid returns generic disambiguate" to "[ty] Improve disambiguation of class names in diagnostics" by @AlexWaygood on 2025-09-27 19:15_

---

_Comment by @github-actions[bot] on 2025-09-27 19:16_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-27 19:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandera (https://github.com/pandera-dev/pandera)
- tests/pandas/test_dtypes.py:170:1: error[invalid-assignment] Object of type `list[Unknown | tuple[dict[Unknown | <class 'int'> | <class 'Int'> | <class 'Int8'> | <class 'Int16'> | <class 'Int32'> | <class 'Int64'> | <class 'signedinteger[_8Bit]'> | <class 'signedinteger[_16Bit]'> | <class 'signedinteger[_32Bit]'> | <class 'signedinteger[_64Bit]'>, Unknown | str], list[Unknown | int]] | tuple[dict[Unknown | <class 'INT8'> | <class 'INT16'> | <class 'INT32'> | <class 'INT64'>, Unknown | str], list[Unknown | int | None]] | tuple[dict[Unknown | <class 'UInt'> | <class 'UInt8'> | <class 'UInt16'> | <class 'UInt32'> | <class 'UInt64'>, Unknown | str], list[Unknown | int]] | tuple[dict[Unknown | <class 'UINT8'> | <class 'UINT16'> | <class 'UINT32'> | <class 'UINT64'>, Unknown | str], list[Unknown | int | None]] | tuple[dict[Unknown | <class 'float'> | <class 'Float'> | <class 'Float16'> | <class 'Float32'> | <class 'Float64'> | <class 'float64'>, Unknown | str], list[Unknown | float]] | tuple[dict[Unknown | <class 'complex'> | <class 'Complex'> | <class 'Complex64'> | <class 'Complex128'>, Unknown | str], list[Unknown | complex]] | tuple[dict[Unknown | <class 'bool'> | <class 'Bool'> | <class 'bool'>, Unknown | str], list[Unknown | bool]] | tuple[dict[Unknown | <class 'BooleanDtype'> | <class 'BOOL'>, Unknown | str], list[Unknown | bool | None]] | tuple[dict[Unknown | <class 'str'> | <class 'String'> | <class 'str_'>, Unknown | str], list[Unknown | str]] | tuple[dict[Unknown | <class 'object'> | <class 'object_'>, Unknown | str], list[Unknown | str]] | tuple[dict[Unknown | <class 'StringDtype'>, Unknown | str], list[Unknown | int | None]] | tuple[dict[Unknown | <class 'Category'> | Category | CategoricalDtype, Unknown | str | CategoricalDtype], list[Unknown | int | None]] | tuple[dict[Unknown | <class 'datetime'> | <class 'datetime64'> | <class 'Timestamp'> | DatetimeTZDtype | <class 'DateTime'> | DateTime, Unknown | str], Unknown] | tuple[dict[Unknown | PeriodDtype, Unknown | str], Unknown] | tuple[dict[Unknown | <class 'SparseDtype'> | SparseDtype, Unknown | SparseDtype], Series[Any]] | tuple[dict[Unknown | IntervalDtype, Unknown | str], Series[Any]]]` is not assignable to `list[tuple[dict[Unknown, Unknown], list[Unknown]]]`
+ tests/pandas/test_dtypes.py:170:1: error[invalid-assignment] Object of type `list[Unknown | tuple[dict[Unknown | <class 'int'> | <class 'Int'> | <class 'Int8'> | <class 'Int16'> | <class 'Int32'> | <class 'Int64'> | <class 'signedinteger[_8Bit]'> | <class 'signedinteger[_16Bit]'> | <class 'signedinteger[_32Bit]'> | <class 'signedinteger[_64Bit]'>, Unknown | str], list[Unknown | int]] | tuple[dict[Unknown | <class 'INT8'> | <class 'INT16'> | <class 'INT32'> | <class 'INT64'>, Unknown | str], list[Unknown | int | None]] | tuple[dict[Unknown | <class 'UInt'> | <class 'UInt8'> | <class 'UInt16'> | <class 'UInt32'> | <class 'UInt64'>, Unknown | str], list[Unknown | int]] | tuple[dict[Unknown | <class 'UINT8'> | <class 'UINT16'> | <class 'UINT32'> | <class 'UINT64'>, Unknown | str], list[Unknown | int | None]] | tuple[dict[Unknown | <class 'float'> | <class 'Float'> | <class 'Float16'> | <class 'Float32'> | <class 'Float64'> | <class 'float64'>, Unknown | str], list[Unknown | float]] | tuple[dict[Unknown | <class 'complex'> | <class 'Complex'> | <class 'Complex64'> | <class 'Complex128'>, Unknown | str], list[Unknown | complex]] | tuple[dict[Unknown | <class 'builtins.bool'> | <class 'Bool'> | <class 'numpy.bool'>, Unknown | str], list[Unknown | builtins.bool]] | tuple[dict[Unknown | <class 'BooleanDtype'> | <class 'BOOL'>, Unknown | str], list[Unknown | builtins.bool | None]] | tuple[dict[Unknown | <class 'str'> | <class 'String'> | <class 'str_'>, Unknown | str], list[Unknown | str]] | tuple[dict[Unknown | <class 'object'> | <class 'object_'>, Unknown | str], list[Unknown | str]] | tuple[dict[Unknown | <class 'StringDtype'>, Unknown | str], list[Unknown | int | None]] | tuple[dict[Unknown | <class 'Category'> | Category | CategoricalDtype, Unknown | str | CategoricalDtype], list[Unknown | int | None]] | tuple[dict[Unknown | <class 'datetime'> | <class 'datetime64'> | <class 'Timestamp'> | DatetimeTZDtype | <class 'DateTime'> | DateTime, Unknown | str], Unknown] | tuple[dict[Unknown | PeriodDtype, Unknown | str], Unknown] | tuple[dict[Unknown | <class 'SparseDtype'> | SparseDtype, Unknown | SparseDtype], Series[Any]] | tuple[dict[Unknown | IntervalDtype, Unknown | str], Series[Any]]]` is not assignable to `list[tuple[dict[Unknown, Unknown], list[Unknown]]]`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/client_options.py:145:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[SSLContext | None, bool]`, found `tuple[SSLContext | SSLContext | Unknown, Any]`
+ pymongo/client_options.py:145:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[pymongo.pyopenssl_context.SSLContext | None, bool]`, found `tuple[pymongo.pyopenssl_context.SSLContext | ssl.SSLContext | Unknown, Any]`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/dist.py:979:20: error[invalid-return-type] Return type does not match returned value: expected `Command`, found `(str & Command) | (Command & Command) | Unknown`
+ setuptools/_distutils/dist.py:979:20: error[invalid-return-type] Return type does not match returned value: expected `setuptools._distutils.cmd.Command`, found `(str & distutils.cmd.Command) | (setuptools._distutils.cmd.Command & distutils.cmd.Command) | Unknown`
- setuptools/_distutils/dist.py:989:16: error[invalid-return-type] Return type does not match returned value: expected `Command`, found `(str & Command) | (Command & Command) | Unknown`
+ setuptools/_distutils/dist.py:989:16: error[invalid-return-type] Return type does not match returned value: expected `setuptools._distutils.cmd.Command`, found `(str & distutils.cmd.Command) | (setuptools._distutils.cmd.Command & distutils.cmd.Command) | Unknown`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[Model]`, found `set[Model]`
+ src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[src.bokeh.model.model.Model]`, found `set[src.bokeh.model.model.Model]`

jax (https://github.com/google/jax)
- jax/_src/mesh_utils.py:552:12: error[invalid-return-type] Return type does not match returned value: expected `builtins.bool`, found `numpy.bool[bool]`
+ jax/_src/mesh_utils.py:552:12: error[invalid-return-type] Return type does not match returned value: expected `builtins.bool`, found `numpy.bool[builtins.bool]`
- jax/_src/mesh_utils.py:582:12: error[invalid-return-type] Return type does not match returned value: expected `builtins.bool`, found `numpy.bool[bool]`
+ jax/_src/mesh_utils.py:582:12: error[invalid-return-type] Return type does not match returned value: expected `builtins.bool`, found `numpy.bool[builtins.bool]`

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-09-27 19:25_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-27 19:25_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-27 19:25_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-27 19:25_

---

_Label `diagnostics` added by @AlexWaygood on 2025-09-27 19:25_

---

_Comment by @sharkdp on 2025-09-29 07:04_

```diff
- src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[Model]`, found `set[Model]`
+ src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[src.bokeh.model.model.Model]`, found `set[src.bokeh.model.model.Model]`
```
obviously not related to your PR, but it looks like something is wrong here?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/display.rs`:83 on 2025-09-29 07:18_

Probably not worth the additional work, but instead of allocating a hash set of all discovered `ClassLiteral`s with that name, you could introduce an `Option`-like enum
```rs
enum AmbiguityState<'db> {
    Unambiguous(ClassLiteral<'db>),
    Ambiguous,
}
```
When you discover the first class literal, you start with `AmbiguityState::Unambiguous(first_literal)`. And as soon as you discover *another* class literal, you simply flip the state to `AmbiguityState::Ambiguous`. At that point, we're not interested anymore in which exact class literals contributed to the ambiguity.

---

_@sharkdp approved on 2025-09-29 07:19_

Thank you very much. Slightly unfortunate that we gain `.clone`s in multiple places now, but this is probably not performance-critical.

---

_Comment by @AlexWaygood on 2025-09-29 10:15_

> ```diff
> - src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[Model]`, found `set[Model]`
> + src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[src.bokeh.model.model.Model]`, found `set[src.bokeh.model.model.Model]`
> ```
> 
> obviously not related to your PR, but it looks like something is wrong here?

Yes, definitely (multiple things, in fact...) -- but at least we now know that this specific case isn't solved by printing the fully qualified name in the diagnostics 😆

---

_@AlexWaygood reviewed on 2025-09-29 10:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:83 on 2025-09-29 10:15_

Ahh, that's a great suggestion, thanks!

---

_Comment by @AlexWaygood on 2025-09-29 10:19_

> Slightly unfortunate that we gain `.clone`s in multiple places now, but this is probably not performance-critical.

Yes, I didn't love this, but couldn't see an easy way to get around it. I think `DisplaySettings` should be fairly cheap to clone, though, since it uses an `Rc<HashSet>` internally rather than a `HashSet`

---

_Merged by @AlexWaygood on 2025-09-29 10:43_

---

_Closed by @AlexWaygood on 2025-09-29 10:43_

---

_Branch deleted on 2025-09-29 10:43_

---
