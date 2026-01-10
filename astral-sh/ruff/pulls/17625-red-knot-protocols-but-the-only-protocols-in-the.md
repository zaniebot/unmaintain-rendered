```yaml
number: 17625
title: "[red-knot] Protocols, but the only protocols in the world are the ones in the `typing` module"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - do-not-merge
  - ty
assignees: []
draft: true
base: main
head: alex/protocol-variant-only-known
created_at: 2025-04-25T12:26:00Z
updated_at: 2025-04-29T14:43:12Z
url: https://github.com/astral-sh/ruff/pull/17625
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Protocols, but the only protocols in the world are the ones in the `typing` module

---

_Pull request opened by @AlexWaygood on 2025-04-25 12:26_

## Summary

This is a somewhat silly version of https://github.com/astral-sh/ruff/pull/17614 where we pretend that the only protocols in the world are the ones that are defined in the typing module. I'm not intending for this to be merged; I'm mainly interested in seeing if this has any impact on mypy_primer or not.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `do-not-merge` added by @AlexWaygood on 2025-04-25 12:26_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-25 12:26_

---

_Renamed from "Protocols, but the only two protocols in the world are `SupportsIndex` and `Sized`" to "[red-knot] Protocols, but the only two protocols in the world are `SupportsIndex` and `Sized`" by @AlexWaygood on 2025-04-25 12:26_

---

_Comment by @github-actions[bot] on 2025-04-25 12:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/zipp/zipp/glob.py:4:26: Operator `*` is unsupported between objects of type `str` and `bool`
- Found 4 diagnostics
+ Found 3 diagnostics

packaging (https://github.com/pypa/packaging)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:478:13: Object of type `Literal[0]` is not assignable to `str | bytes | SupportsInt | None`
- Found 27 diagnostics
+ Found 26 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/_paramkey.py:62:12: Return type does not match returned value: Expected `Hashable`, found `_HK`
- Found 32 diagnostics
+ Found 31 diagnostics

pip (https://github.com/pypa/pip)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pip/src/pip/_internal/req/req_file.py:182:29: Argument to this function is incorrect: Expected `Iterable`, found `enumerate`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/pip/src/pip/_vendor/packaging/version.py:478:13: Object of type `Literal[0]` is not assignable to `str | bytes | SupportsInt | None`
- Found 1097 diagnostics
+ Found 1095 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:880:20: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1437:44: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1557:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1558:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1577:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1578:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:3823:16: Return type does not match returned value: Expected `Hashable`, found `tuple[Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown | None]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1551:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1553:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/syzygy.py:446:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/syzygy.py:447:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/variant.py:132:20: Return type does not match returned value: Expected `Hashable`, found `tuple[Unknown, Unknown]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:682:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:683:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:686:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:687:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/variant.py:799:16: Return type does not match returned value: Expected `Hashable`, found `tuple[Unknown, Unknown, Unknown]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:829:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:830:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:833:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:834:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/variant.py:934:16: Return type does not match returned value: Expected `Hashable`, found `tuple[Unknown, Unknown, str, str]`
- Found 81 diagnostics
+ Found 53 diagnostics

beartype (https://github.com/beartype/beartype)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/beartype/beartype/_check/convert/_convcoerce.py:409:13: Argument to this function is incorrect: Expected `Hashable`, found `str`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/beartype/beartype/_util/cache/pool/utilcachepool.py:136:9: Default value of type `None` is not assignable to annotated parameter type `Hashable`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/beartype/beartype/_util/cache/pool/utilcachepool.py:242:9: Default value of type `None` is not assignable to annotated parameter type `Hashable`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/beartype/beartype/_util/cache/pool/utilcachepoolinstance.py:115:33: Argument to this function is incorrect: Expected `Hashable`, found `type[Any]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/beartype/beartype/_util/cache/pool/utilcachepoollistfixed.py:345:43: Argument to this function is incorrect: Expected `Hashable`, found `int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/beartype/beartype/_util/cache/pool/utilcachepoollistfixed.py:387:30: Argument to this function is incorrect: Expected `Hashable`, found `int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/beartype/beartype/door/_cls/doormeta.py:165:17: Argument to this function is incorrect: Expected `Hashable`, found `str | int`
- Found 574 diagnostics
+ Found 567 diagnostics

ppb-vector (https://github.com/ppb/ppb-vector)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:291:9: Object of type `float` is not assignable to `SupportsFloat`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:351:9: Object of type `float` is not assignable to `SupportsFloat`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:436:17: Default value of type `float` is not assignable to annotated parameter type `SupportsFloat`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:436:49: Default value of type `float` is not assignable to annotated parameter type `SupportsFloat`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:459:9: Object of type `float` is not assignable to `SupportsFloat`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:459:18: Object of type `float` is not assignable to `SupportsFloat`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:460:12: Operator `<` is not supported for types `SupportsFloat` and `int`, in comparing `SupportsFloat` with `Literal[0]`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:460:27: Operator `<` is not supported for types `SupportsFloat` and `int`, in comparing `SupportsFloat` with `Literal[0]`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:548:9: Object of type `float` is not assignable to `SupportsFloat`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:560:9: Object of type `float` is not assignable to `SupportsFloat`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:561:12: Operator `<` is not supported for types `SupportsFloat` and `int`, in comparing `SupportsFloat` with `Literal[0]`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:622:17: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:623:17: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/ppb_vector/__init__.py:624:15: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/benchmark.py:9:9: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/benchmark.py:10:9: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_decompose.py:12:8: No overload of function `__new__` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_dot.py:41:36: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_dot.py:41:54: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_isclose.py:90:22: Argument to this function is incorrect: Expected `SupportsFloat`, found `Literal[-1]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_isclose.py:93:22: Argument to this function is incorrect: Expected `SupportsFloat`, found `Literal[-1]`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_member_access.py:9:9: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_pattern_matching.py:5:11: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_pattern_matching.py:13:11: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_pattern_matching.py:21:11: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_pattern_matching.py:29:11: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_project.py:10:8: No overload of function `__new__` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_project.py:24:66: Argument to this function is incorrect: Expected `SupportsFloat`, found `Literal[90]`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:13:6: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:13:25: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:14:6: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:14:23: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:15:6: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:15:24: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:16:6: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:16:25: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:78:6: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:81:6: No overload of function `__new__` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:129:25: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:130:25: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:132:39: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:145:15: No overload of function `__new__` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:155:40: Argument to this function is incorrect: Expected `SupportsFloat`, found `float`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:182:12: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_rotate.py:182:34: No overload of function `__new__` matches arguments
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ppb-vector/tests/test_scalar_multiplication.py:39:13: Operator `/` is unsupported between objects of type `Vector` and `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_scalar_multiplication.py:51:37: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ppb-vector/tests/test_scalar_multiplication.py:62:12: Operator `/` is unsupported between objects of type `Vector` and `int`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ppb-vector/tests/test_scalar_multiplication.py:62:26: Operator `/` is unsupported between objects of type `Vector` and `float`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ppb-vector/tests/test_scalar_multiplication.py:68:9: Operator `/` is unsupported between objects of type `Vector` and `Literal[0]`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ppb-vector/tests/test_scalar_multiplication.py:71:9: Operator `/` is unsupported between objects of type `Vector` and `float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_scale.py:13:20: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_scale.py:20:31: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_scale.py:27:37: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_truncate.py:11:23: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_truncate.py:17:23: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/ppb-vector/tests/test_truncate.py:22:7: No overload of function `__new__` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_truncate.py:35:31: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_truncate.py:40:28: Argument to this function is incorrect: Expected `SupportsFloat`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_truncate.py:48:40: Argument to this function is incorrect: Expected `SupportsFloat`, found `Literal[0]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_truncate.py:48:51: Argument to this function is incorrect: Expected `SupportsFloat`, found `float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_update.py:9:21: Argument to this function is incorrect: Expected `SupportsFloat | None`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_update.py:14:21: Argument to this function is incorrect: Expected `SupportsFloat | None`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_update.py:19:21: Argument to this function is incorrect: Expected `SupportsFloat | None`, found `int | float`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/ppb-vector/tests/test_update.py:19:26: Argument to this function is incorrect: Expected `SupportsFloat | None`, found `int | float`
- Found 151 diagnostics
+ Found 86 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/dedupe/dedupe/convenience.py:57:12: Return type does not match returned value: Expected `Iterator`, found `zip`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/dedupe/dedupe/convenience.py:77:12: Return type does not match returned value: Expected `Iterator`, found `zip`
- Found 127 diagnostics
+ Found 125 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/types.py:3228:17: Argument to this function is incorrect: Expected `str | ((Any, /) -> Hashable)`, found `def _get_type_name(x: Any) -> str`
- Found 923 diagnostics
+ Found 922 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:46:5: Argument to this function is incorrect: Expected `SupportsInt`, found `int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:49:5: Argument to this function is incorrect: Expected `SupportsInt | None`, found `int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:55:5: Argument to this function is incorrect: Expected `SupportsInt`, found `int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:58:5: Argument to this function is incorrect: Expected `SupportsInt | None`, found `int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:146:32: Argument to this function is incorrect: Expected `SupportsInt`, found `Literal[8]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:146:63: Argument to this function is incorrect: Expected `SupportsInt | None`, found `Literal[0]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:165:48: Argument to this function is incorrect: Expected `SupportsInt`, found `Literal[32]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:165:52: Argument to this function is incorrect: Expected `SupportsInt`, found `Literal[32]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:185:13: Argument to this function is incorrect: Expected `SupportsInt`, found `Unknown | int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:188:13: Argument to this function is incorrect: Expected `SupportsInt | None`, found `Unknown | int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:216:53: Argument to this function is incorrect: Expected `SupportsInt`, found `Unknown | int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:552:13: Argument to this function is incorrect: Expected `SupportsInt`, found `Unknown | int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:555:13: Argument to this function is incorrect: Expected `SupportsInt | None`, found `Unknown | int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/tools/tlbparser.py:574:53: Argument to this function is incorrect: Expected `SupportsInt`, found `Unknown | int`
- Found 711 diagnostics
+ Found 697 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/arviz/arviz/tests/base_tests/test_data.py:1582:52: Argument to this function is incorrect: Expected `Hashable | None`, found `Literal["plot"]`
- Found 2521 diagnostics
+ Found 2520 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/ignite/examples/fast_neural_style/handlers.py:25:38: Operator `*` is unsupported between objects of type `Literal[">"]` and `bool`
- Found 2465 diagnostics
+ Found 2464 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/aglib/disasm/arch.py:208:75: Argument to this function is incorrect: Expected `int | SupportsIndex`, found `int | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/aglib/disasm/arch.py:208:75: Argument to this function is incorrect: Expected `SupportsIndex`, found `int | None`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/pwndbg/pwndbg/aglib/nearpc.py:304:24: Operator `*` is unsupported between objects of type `Unknown | Parameter` and `Literal[" "]`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/context.py:1080:39: Operator `*` is unsupported between objects of type `Literal[" "]` and `Parameter`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/radare2.py:43:35: Argument to this function is incorrect: Expected `int | SupportsIndex`, found `int | None | Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/radare2.py:43:35: Argument to this function is incorrect: Expected `SupportsIndex`, found `int | None | Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/rizin.py:41:35: Argument to this function is incorrect: Expected `int | SupportsIndex`, found `int | None | Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pwndbg/pwndbg/commands/rizin.py:41:35: Argument to this function is incorrect: Expected `SupportsIndex`, found `int | None | Unknown`
- Found 2805 diagnostics
+ Found 2803 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:62:29: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[Literal["false"], Literal["true"]]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:65:65: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[Literal["false"], Literal["true"]]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:79:29: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[Literal["false"], Literal["true"]]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:82:65: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[Literal["false"], Literal["true"]]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:168:36: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[Literal["false"], Literal["true"]]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:169:37: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[Literal["false"], Literal["true"]]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:170:33: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[Literal["false"], Literal["true"]]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:171:37: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[Literal["false"], Literal["true"]]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:173:34: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[Literal["false"], Literal["true"]]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_metrics.py:189:58: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- Found 6910 diagnostics
+ Found 6900 diagnostics

rotki (https://github.com/rotki/rotki)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rotki/rotkehlchen/utils/misc.py:308:12: Return type does not match returned value: Expected `Iterator`, found `zip`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rotki/rotkehlchen/utils/misc.py:319:12: Return type does not match returned value: Expected `Iterator`, found `zip_longest`
- Found 2139 diagnostics
+ Found 2137 diagnostics

zulip (https://github.com/zulip/zulip)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/zulip/zerver/signals.py:62:27: Argument to this function is incorrect: Expected `Hashable | None`, found `Literal["only_on_login"]`
- Found 4348 diagnostics
+ Found 4347 diagnostics

jax (https://github.com/google/jax)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/jax/jax/_src/ad_checkpoint.py:596:17: Operator `*` is unsupported between objects of type `Literal["saving inputs with shapes:\n"]` and `bool`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/jax/jax/_src/ad_checkpoint.py:598:17: Operator `*` is unsupported between objects of type `Literal["and "]` and `bool`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/jax/jax/_src/ad_checkpoint.py:599:17: Operator `*` is unsupported between objects of type `Literal["saving these intermediates:\n"]` and `bool`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/jax/jax/_src/api.py:1070:3: Object of type `object` is not assignable to `Hashable | None`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/jax/jax/_src/extend/random.py:32:10: Return type does not match returned value: Expected `Hashable`, found `PRNGSpec`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/jax/jax/_src/lax/parallel.py:135:7: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[list, list]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/jax/jax/_src/lax/parallel.py:772:5: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[list, list]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/jax/jax/_src/lax/parallel.py:773:5: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[list, list]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/jax/jax/_src/lax/parallel.py:926:5: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[list, list]`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/jax/jax/_src/lax/parallel.py:981:5: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[list, list]`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/jax/jax/experimental/pallas/ops/tpu/ragged_paged_attention/kernel.py:108:12: No overload of function `__new__` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/jax/jax/experimental/pallas/ops/tpu/ragged_paged_attention/kernel.py:193:12: No overload of function `__new__` matches arguments
- Found 5499 diagnostics
+ Found 5487 diagnostics

setuptools (https://github.com/pypa/setuptools)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/compilers/C/tests/test_base.py:17:20: Operator `*` is unsupported between objects of type `tuple[Literal["windows.h"]]` and `bool`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/dist.py:377:18: Operator `*` is unsupported between objects of type `Literal["."]` and `bool`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/tests/test_dist.py:21:19: Operator `*` is unsupported between objects of type `Literal["."]` and `bool`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/importlib_metadata/__init__.py:620:20: Operator `*` is unsupported between objects of type `Literal[" "]` and `bool`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/inflect/__init__.py:3915:20: Operator `*` is unsupported between objects of type `list` and `bool`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/packaging/version.py:478:13: Object of type `Literal[0]` is not assignable to `str | bytes | SupportsInt | None`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/wheel/vendored/packaging/version.py:459:13: Object of type `Literal[0]` is not assignable to `str | bytes | SupportsInt | None`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/_vendor/zipp/glob.py:5:26: Operator `*` is unsupported between objects of type `str` and `bool`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/setuptools/setuptools/package_index.py:885:40: Operator `*` is unsupported between objects of type `list` and `bool`
- Found 1084 diagnostics
+ Found 1075 diagnostics

```
</details>


---

_Renamed from "[red-knot] Protocols, but the only two protocols in the world are `SupportsIndex` and `Sized`" to "[red-knot] Protocols, but the only two protocols in the world are the ones in the `typing` module" by @AlexWaygood on 2025-04-25 13:03_

---

_Renamed from "[red-knot] Protocols, but the only two protocols in the world are the ones in the `typing` module" to "[red-knot] Protocols, but the only protocols in the world are the ones in the `typing` module" by @AlexWaygood on 2025-04-25 13:06_

---

_Closed by @AlexWaygood on 2025-04-29 14:43_

---

_Branch deleted on 2025-04-29 14:43_

---
