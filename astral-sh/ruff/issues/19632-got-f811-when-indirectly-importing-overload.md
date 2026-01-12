```yaml
number: 19632
title: Got F811 when indirectly importing overload
type: issue
state: open
author: rj-xu
labels:
  - bug
  - rule
assignees: []
created_at: 2025-07-30T09:22:26Z
updated_at: 2025-08-01T17:08:57Z
url: https://github.com/astral-sh/ruff/issues/19632
synced_at: 2026-01-12T15:54:57Z
```

# Got F811 when indirectly importing overload

---

_@rj-xu_

### Summary

```python
from std import overload


@overload
def func(a: str, b: int) -> int: ...
@overload
def func(a: int, b: str) -> int: ...
def func(a: int | str, b: int | str) -> int:
    return 0

```

```toml
typing-modules = ["std"]
```

#6969 
In version 0.12.5, `F811 Redefinition` still occurs, even though `typing-modules` have already been set up.
This error will appear after v0.3.0.

```shell
ruff --version
ruff 0.3.0 (b53118ed0 2024-02-29)
ruff check .\src\temp.py
src\temp.py:1:1: INP001 File `src\temp.py` is part of an implicit namespace package. Add an `__init__.py`.
src\temp.py:8:10: ARG001 Unused function argument: `a`
src\temp.py:8:24: ARG001 Unused function argument: `b`
Found 3 errors.
```

```shell
ruff --version
ruff 0.3.1 (b9264a5a1 2024-03-06)
ruff check .\src\temp.py
src\temp.py:1:1: INP001 File `src\temp.py` is part of an implicit namespace package. Add an `__init__.py`.
src\temp.py:5:10: ARG001 Unused function argument: `a`
src\temp.py:5:18: ARG001 Unused function argument: `b`
src\temp.py:7:5: F811 Redefinition of unused `func` from line 5
src\temp.py:7:10: ARG001 Unused function argument: `a`
src\temp.py:7:18: ARG001 Unused function argument: `b`
src\temp.py:8:5: F811 Redefinition of unused `func` from line 7
src\temp.py:8:10: ARG001 Unused function argument: `a`
src\temp.py:8:24: ARG001 Unused function argument: `b`
Found 9 errors.
```
```shell
ruff --version
ruff 0.12.5 (d13228ab8 2025-07-24)
ruff check .\src\temp.py
src\temp.py:7:5: F811 Redefinition of unused `func` from line 5
  |
5 | def func(a: str, b: int) -> int: ...
6 | @overload
7 | def func(a: int, b: str) -> int: ...
  |     ^^^^ F811
8 | def func(a: int | str, b: int | str) -> int:
9 |     return 0
  |
  = help: Remove definition: `func`

src\temp.py:8:5: F811 Redefinition of unused `func` from line 7
  |
6 | @overload
7 | def func(a: int, b: str) -> int: ...
8 | def func(a: int | str, b: int | str) -> int:
  |     ^^^^ F811
9 |     return 0
  |
  = help: Remove definition: `func`

src\temp.py:8:10: ARG001 Unused function argument: `a`
  |
6 | @overload
7 | def func(a: int, b: str) -> int: ...
8 | def func(a: int | str, b: int | str) -> int:
  |          ^ ARG001
9 |     return 0
  |

src\temp.py:8:24: ARG001 Unused function argument: `b`
  |
6 | @overload
7 | def func(a: int, b: str) -> int: ...
8 | def func(a: int | str, b: int | str) -> int:
  |                        ^ ARG001
9 |     return 0
  |

Found 4 errors.
```


### Version

ruff 0.12.5 (d13228ab8 2025-07-24)

---

_Comment by @ntBre on 2025-07-30 13:00_

Thanks for opening the issue! I can reproduce this in the [playground](https://play.ruff.rs/10fc6710-1db3-431d-90d2-eaec9d01140c). I can reproduce it for the code in #6969 too, so maybe there has been a regression since then, just like you said. This command:

```shell
uvx ruff@0.3.0 check --no-cache --select F811 --config 'lint.typing-modules=["std"]'
```

gives no diagnostics, but 0.4.0 and 0.12.7 both reproduce your output above. I'm starting to bisect now.

Edit: Bisected to https://github.com/astral-sh/ruff/pull/10210

---

_Label `bug` added by @ntBre on 2025-07-30 13:00_

---

_Label `rule` added by @ntBre on 2025-07-30 13:00_

---

_Comment by @rj-xu on 2025-08-01 09:00_

`Final` doesn't work either.

---

_Comment by @ntBre on 2025-08-01 17:08_

I think this is actually somewhat related to https://github.com/astral-sh/ruff/issues/19664. `QualifiedName::from_dotted_name` is involved again here:

https://github.com/astral-sh/ruff/blob/dbd067c8698837044f505e7b12e1f733dedec46d/crates/ruff_python_semantic/src/model.rs#L210-L215

In fact, if you change the input to this (import from `std.std`):

```
from std.std import overload


@overload
def func(a: str, b: int) -> int: ...
@overload
def func(a: int, b: str) -> int: ...
def func(a: int | str, b: int | str) -> int:
    return 0
```

and pass `lint.typing-modules=["std.std"]`, it works as expected ([playground](https://play.ruff.rs/395a1762-a31e-4881-98f9-118fd7e451ea)).

---
