```yaml
number: 11512
title: TRY300 and RET506 sometimes conflicts
type: issue
state: closed
author: tyilo
labels: []
assignees: []
created_at: 2024-05-23T11:14:06Z
updated_at: 2024-05-23T12:18:16Z
url: https://github.com/astral-sh/ruff/issues/11512
synced_at: 2026-01-12T15:54:51Z
```

# TRY300 and RET506 sometimes conflicts

---

_@tyilo_

Example code:

```python3
class FooError(Exception):
    pass


class BarError(Exception):
    pass


def foo(*, x: bool) -> int:
    try:
        if x:
            raise FooError
        else:
            return 1
    except BarError:
        return 2
```

```
$ ruff --version
ruff 0.4.5
$ ruff check --isolated --select ALL --ignore 'D' a.py
a.py:13:9: RET506 Unnecessary `else` after `raise` statement
Found 1 error.
```

Removing the `else` statement:
```
$ ruff check --isolated --select ALL --ignore 'D' a.py
a.py:13:9: TRY300 Consider moving this statement to an `else` block
Found 1 error.
```

---

_Comment by @tyilo on 2024-05-23 12:18_

Ah, `TRY300` refers to the `else` block of `try ... except ... else ...` and not `if ... else ...`.

So `TRY300` wants me to refactor the code to:
```python3
class FooError(Exception):
    pass


class BarError(Exception):
    pass


def foo(*, x: bool) -> int:
    try:
        if x:
            raise FooError
    except BarError:
        return 2
    else:
        return 1
```

which then fixes all lints.

---

_Closed by @tyilo on 2024-05-23 12:18_

---
