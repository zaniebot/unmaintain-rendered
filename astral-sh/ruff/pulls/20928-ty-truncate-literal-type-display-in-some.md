```yaml
number: 20928
title: "[ty] Truncate Literal type display in some situations"
type: pull_request
state: merged
author: InvalidPathException
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: invalid/truncate-wide-literals
created_at: 2025-10-16T19:46:35Z
updated_at: 2025-10-17T11:50:58Z
url: https://github.com/astral-sh/ruff/pull/20928
synced_at: 2026-01-12T15:57:12Z
```

# [ty] Truncate Literal type display in some situations

---

_@InvalidPathException_

## Summary

Fixes [astral-sh/ty#1369](https://github.com/astral-sh/ty/issues/1369)

## Test Plan

A new mdtest case is created
`Union[Literal[1, 2, 3, 4, 5, 6, 7, 8], A, B, C, D, E, F]` -> `Literal[1, 2, 3, 4, 5, ... omitted 3 literals] | A | B | ... omitted 4 union elements` because there was no appropriate existing case.

The non-truncation case for reveal_type has many existing tests to verify, for example, `crates/ty_python_semantic/resources/mdtest/subscript/tuple.md`.

Tentatively setting truncation threshold to 7. When truncated, we keep the first 5 elements.

---

_Review requested from @carljm by @InvalidPathException on 2025-10-16 19:46_

---

_Review requested from @AlexWaygood by @InvalidPathException on 2025-10-16 19:46_

---

_Review requested from @sharkdp by @InvalidPathException on 2025-10-16 19:46_

---

_Review requested from @dcreager by @InvalidPathException on 2025-10-16 19:46_

---

_Comment by @github-actions[bot] on 2025-10-16 19:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-16 19:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
colour (https://github.com/colour-science/colour)
- colour/geometry/tests/test_primitives.py:782:32: error[invalid-argument-type] Argument to function `primitive_cube` is incorrect: Expected `Literal["-x", "+x", "-y", "+y", "-z", "+z", "xy", "xz", "yz", "yx", "zx", "zy"] | None`, found `list[Unknown]`
+ colour/geometry/tests/test_primitives.py:782:32: error[invalid-argument-type] Argument to function `primitive_cube` is incorrect: Expected `Literal["-x", "+x", "-y", "+y", "-z", ... omitted 7 literals] | None`, found `list[Unknown]`
- colour/geometry/tests/test_primitives.py:783:32: error[invalid-argument-type] Argument to function `primitive_cube` is incorrect: Expected `Literal["-x", "+x", "-y", "+y", "-z", "+z", "xy", "xz", "yz", "yx", "zx", "zy"] | None`, found `list[Unknown]`
+ colour/geometry/tests/test_primitives.py:783:32: error[invalid-argument-type] Argument to function `primitive_cube` is incorrect: Expected `Literal["-x", "+x", "-y", "+y", "-z", ... omitted 7 literals] | None`, found `list[Unknown]`
- colour/geometry/tests/test_vertices.py:257:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", "+z", "xy", "xz", "yz", "yx", "zx", "zy"]] | None`, found `list[Unknown | str]`
+ colour/geometry/tests/test_vertices.py:257:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", ... omitted 7 literals]] | None`, found `list[Unknown | str]`
- colour/geometry/tests/test_vertices.py:263:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", "+z", "xy", "xz", "yz", "yx", "zx", "zy"]] | None`, found `list[Unknown | str]`
+ colour/geometry/tests/test_vertices.py:263:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", ... omitted 7 literals]] | None`, found `list[Unknown | str]`
- colour/geometry/tests/test_vertices.py:269:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", "+z", "xy", "xz", "yz", "yx", "zx", "zy"]] | None`, found `list[Unknown | str]`
+ colour/geometry/tests/test_vertices.py:269:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", ... omitted 7 literals]] | None`, found `list[Unknown | str]`
- colour/geometry/tests/test_vertices.py:275:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", "+z", "xy", "xz", "yz", "yx", "zx", "zy"]] | None`, found `list[Unknown | str]`
+ colour/geometry/tests/test_vertices.py:275:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", ... omitted 7 literals]] | None`, found `list[Unknown | str]`
- colour/geometry/tests/test_vertices.py:281:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", "+z", "xy", "xz", "yz", "yx", "zx", "zy"]] | None`, found `list[Unknown | str]`
+ colour/geometry/tests/test_vertices.py:281:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", ... omitted 7 literals]] | None`, found `list[Unknown | str]`
- colour/geometry/tests/test_vertices.py:287:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", "+z", "xy", "xz", "yz", "yx", "zx", "zy"]] | None`, found `list[Unknown | str]`
+ colour/geometry/tests/test_vertices.py:287:41: error[invalid-argument-type] Argument to function `primitive_vertices_cube_mpl` is incorrect: Expected `Sequence[Literal["-x", "+x", "-y", "+y", "-z", ... omitted 7 literals]] | None`, found `list[Unknown | str]`

pywin32 (https://github.com/mhammond/pywin32)
- win32/test/test_win32api.py:110:42: error[invalid-argument-type] Argument to function `RegSetValueEx` is incorrect: Expected `str`, found `None | Literal["REG_SZ", "REG_EXPAND_SZ", "REG_MULTI_SZ", "REG_MULTI_SZ_empty", "REG_DWORD", "REG_QWORD_INT", "REG_QWORD", "REG_BINARY"]`
+ win32/test/test_win32api.py:110:42: error[invalid-argument-type] Argument to function `RegSetValueEx` is incorrect: Expected `str`, found `None | Literal["REG_SZ", "REG_EXPAND_SZ", "REG_MULTI_SZ", "REG_MULTI_SZ_empty", "REG_DWORD", ... omitted 3 literals]`

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/impala/tests/test_exprs.py:198:60: error[invalid-argument-type] Argument to bound method `truncate` is incorrect: Expected `Literal["Y", "Q", "M", "W", "D", "h", "m", "s", "ms", "us", "ns"]`, found `Literal["y"]`
+ ibis/backends/impala/tests/test_exprs.py:198:60: error[invalid-argument-type] Argument to bound method `truncate` is incorrect: Expected `Literal["Y", "Q", "M", "W", "D", ... omitted 6 literals]`, found `Literal["y"]`
- ibis/backends/impala/tests/test_exprs.py:206:60: error[invalid-argument-type] Argument to bound method `truncate` is incorrect: Expected `Literal["Y", "Q", "M", "W", "D", "h", "m", "s", "ms", "us", "ns"]`, found `Literal["month"]`
+ ibis/backends/impala/tests/test_exprs.py:206:60: error[invalid-argument-type] Argument to bound method `truncate` is incorrect: Expected `Literal["Y", "Q", "M", "W", "D", ... omitted 6 literals]`, found `Literal["month"]`
- ibis/backends/impala/tests/test_exprs.py:210:60: error[invalid-argument-type] Argument to bound method `truncate` is incorrect: Expected `Literal["Y", "Q", "M", "W", "D", "h", "m", "s", "ms", "us", "ns"]`, found `Literal["d"]`
+ ibis/backends/impala/tests/test_exprs.py:210:60: error[invalid-argument-type] Argument to bound method `truncate` is incorrect: Expected `Literal["Y", "Q", "M", "W", "D", ... omitted 6 literals]`, found `Literal["d"]`
- ibis/backends/impala/tests/test_exprs.py:222:60: error[invalid-argument-type] Argument to bound method `truncate` is incorrect: Expected `Literal["Y", "Q", "M", "W", "D", "h", "m", "s", "ms", "us", "ns"]`, found `Literal["minute"]`
+ ibis/backends/impala/tests/test_exprs.py:222:60: error[invalid-argument-type] Argument to bound method `truncate` is incorrect: Expected `Literal["Y", "Q", "M", "W", "D", ... omitted 6 literals]`, found `Literal["minute"]`

```
</details>
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-10-16 20:09_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-16 20:09_

---

_Review request for @sharkdp removed by @sharkdp on 2025-10-17 11:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:1325 on 2025-10-17 11:42_

```suggestion
#[derive(Debug, Copy, Clone)]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:1346 on 2025-10-17 11:43_

```suggestion
#[derive(Debug)]
struct DisplayOmitted {
```

---

_@AlexWaygood approved on 2025-10-17 11:45_

Excellent, thank you!

---

_Merged by @AlexWaygood on 2025-10-17 11:50_

---

_Closed by @AlexWaygood on 2025-10-17 11:50_

---
