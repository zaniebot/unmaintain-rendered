---
number: 14231
title: "UP044 has unsafe or incorrect fixes when `Unpack` is used in unusual ways"
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2024-11-09T18:43:08Z
updated_at: 2024-11-09T20:47:29Z
url: https://github.com/astral-sh/ruff/issues/14231
synced_at: 2026-01-10T01:22:54Z
---

# UP044 has unsafe or incorrect fixes when `Unpack` is used in unusual ways

---

_Issue opened by @dscorbett on 2024-11-09 18:43_

The fixes for [`non-pep646-unpack` (UP044)](https://docs.astral.sh/ruff/rules/non-pep646-unpack/) have three problems in Ruff 0.7.3 when `Unpack` is used in unusual ways. One problem changes behavior, one introduces a syntax error, and one makes Ruff exit with an error.

First, `Unpack` and `*` produce different values at runtime, so the fix changes behavior. In particular, if it is being used as an annotation, the change could affect runtime type introspection. It is also possible to use it outside an annotation. When it makes a difference at runtime, the fix should either be marked unsafe or suppressed.
```console
$ cat up044_1.py
from typing import Unpack
def f(*args: Unpack[tuple[int, ...]]) -> tuple[int, ...]:
    return args
print(f.__annotations__["args"])
print(Unpack[tuple[int, ...]])

$ python up044_1.py
typing.Unpack[tuple[int, ...]]
typing.Unpack[tuple[int, ...]]

$ ruff check --target-version py311 --isolated --preview --select UP044 up044_1.py --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat up044_1.py
from typing import Unpack
def f(*args: *tuple[int, ...]) -> tuple[int, ...]:
    return args
print(f.__annotations__["args"])
print(*tuple[int, ...])

$ python up044_1.py
*tuple[int, ...]
*tuple[int, ...]
```

Second, the `*` is a syntax error unless in an annotation or within brackets. For example, it is not valid after `return`.
```console
$ cat up044_2.py
from typing import Unpack
def f() -> object:
    return Unpack[tuple[int, ...]]
print(f())

$ python up044_2.py
typing.Unpack[tuple[int, ...]]

$ ruff check --target-version py311 --isolated --preview --select UP044 up044_2.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat up044_2.py
from typing import Unpack
def f() -> object:
    return *tuple[int, ...]
print(f())

$ python up044_2.py
  File "up044_2.py", line 3
    return *tuple[int, ...]
           ^^^^^^^^^^^^^^^^
SyntaxError: can't use starred expression here
```

Third, if `Unpack` is misused as an annotation for a non-variadic parameter, Ruff exits with an error.
```console
$ cat up044_3.py
from typing import Unpack
def f(x: Unpack[tuple[int, ...]]) -> object:
    return x
print(f(()))

$ python up044_3.py
()

$ ruff check --target-version py311 --isolated --preview --select UP044 up044_3.py --fix

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `up044_3.py`, the rule codes UP044, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

up044_3.py:2:10: UP044 Use `*` for unpacking
  |
1 | from typing import Unpack
2 | def f(x: Unpack[tuple[int, ...]]) -> object:
  |          ^^^^^^^^^^^^^^^^^^^^^^^ UP044
3 |     return x
4 | print(f(()))
  |
  = help: Convert to `*` for unpacking

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Label `bug` added by @charliermarsh on 2024-11-09 19:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 20:12_

---

_Referenced in [astral-sh/ruff#14234](../../astral-sh/ruff/pulls/14234.md) on 2024-11-09 20:30_

---

_Closed by @charliermarsh on 2024-11-09 20:47_

---
