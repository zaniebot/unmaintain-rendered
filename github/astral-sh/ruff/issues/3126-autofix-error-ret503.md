---
number: 3126
title: "[Autofix error] RET503"
type: issue
state: closed
author: hofrob
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-02-22T14:17:17Z
updated_at: 2023-02-22T19:36:16Z
url: https://github.com/astral-sh/ruff/issues/3126
synced_at: 2026-01-07T13:12:14-06:00
---

# [Autofix error] RET503

---

_Issue opened by @hofrob on 2023-02-22 14:17_

# versions

```
$ python --version
Python 3.11.2
$ ruff --version
ruff 0.0.251
```

# example 

```py
def foo(bar: list[str]) -> str | None:
    for baz in bar:
        match baz:
            case "yo":
                return sub_foo(baz)


def sub_foo(baz: str) -> str:
    return baz
```

# failing command

(there's no `pyproject.toml`)

```
$ ruff check -n --fix --select RET503 .

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `foo.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

foo.py:2:5: RET503 Missing explicit `return` at the end of function able to return non-`None` value
Found 1 error.
```


---

_Label `bug` added by @charliermarsh on 2023-02-22 14:25_

---

_Label `autofix` added by @charliermarsh on 2023-02-22 14:25_

---

_Comment by @charliermarsh on 2023-02-22 14:25_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-22 15:27_

---

_Referenced in [RustPython/RustPython#4552](../../RustPython/RustPython/pulls/4552.md) on 2023-02-22 16:27_

---

_Referenced in [astral-sh/ruff#3141](../../astral-sh/ruff/pulls/3141.md) on 2023-02-22 19:29_

---

_Closed by @charliermarsh on 2023-02-22 19:36_

---
