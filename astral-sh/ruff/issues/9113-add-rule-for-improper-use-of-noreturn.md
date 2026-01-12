```yaml
number: 9113
title: "Add rule for improper use of `NoReturn` "
type: issue
state: closed
author: SRv6d
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-12-13T11:59:28Z
updated_at: 2024-04-03T14:04:31Z
url: https://github.com/astral-sh/ruff/issues/9113
synced_at: 2026-01-12T15:54:48Z
```

# Add rule for improper use of `NoReturn` 

---

_@SRv6d_

The `NoReturn` return type is commonly misunderstood and not very intuitive. 

It should be used for functions that **never** return, either through unconditionally raising an exception, 
or by exiting.

Yet, I commonly see it misused (and have misused it myself) to annotate functions that may raise or return using a union return value, eg:
```python
def raise_conditionally(raise_exc: bool) -> None | NoReturn:
    if raise_exc:
        raise RuntimeError("raise_exc was true")
    return None
```

Although ideally ruff would ensure the full invariant(no use of `NoReturn` for functions that might do anything other than raise or exit), I haven't looked into how much work it would be to implement that and simply ensuring no use of union types with `NoReturn` would go a long way.

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `rule` added by @charliermarsh on 2023-12-13 15:48_

---

_Label `needs-decision` added by @charliermarsh on 2023-12-13 15:48_

---

_Comment by @Avasam on 2023-12-14 06:52_

This could fit https://github.com/PyCQA/flake8-pyi (which Ruff re-implements). I have myself introduced this incorrect union once by accident in typeshed (see https://github.com/python/typeshed/pull/10819 )

---

_Comment by @andersk on 2023-12-15 23:24_

All Ruff needs to check is that `typing.NoReturn` (and `typing.Never`) shouldn’t appear in a union, since that’s redundant: `T | NoReturn` is exactly equivalent to `T`.

Mypy already checks that `NoReturn` is not misused outside a union for a function that returns, and Ruff can’t do that without type information (since a function that always calls other `-> NoReturn` functions can be `-> NoReturn`).

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-20 17:42_

---

_Label `needs-decision` removed by @charliermarsh on 2023-12-20 17:42_

---

_Label `accepted` added by @charliermarsh on 2023-12-20 17:42_

---

_Comment by @charliermarsh on 2023-12-20 17:52_

Will add.

---

_Closed by @charliermarsh on 2023-12-21 20:53_

---

_Comment by @SRv6d on 2023-12-22 13:04_

@charliermarsh Much appreciated!

---

_Comment by @SRv6d on 2024-02-16 16:55_

@charliermarsh Just FYI, I've just noticed that mypy as well as pylance honor usage of unions containing `NoReturn` in combination with overloads and will warn about code paths that follow a function call with a conditional `NoReturn` return value.

Given the following function:
```python
from typing import Literal, NoReturn, overload


@overload
def raise_conditionally(raise_exc: Literal[False]) -> None:
    ...


@overload
def raise_conditionally(raise_exc: Literal[True]) -> NoReturn:
    ...


def raise_conditionally(raise_exc: bool) -> None | NoReturn:
    if raise_exc:
        raise RuntimeError("raise_exc was true")
    return None
```

This function is fine:
```python
def is_reachable() -> None:
    raise_conditionally(False)
    print("This line will be executed")
```

But in this function the last line will be marked as unreachable:
```python
def is_unreachable() -> None:
    raise_conditionally(True)
    print("This line will not be executed")
```

As per my understanding, this usage is incorrect, but still supported by both type checkers. Knowing this, do you want to keep the `never-union` rules as-is?

---

_Comment by @andersk on 2024-02-16 18:03_

@SRv6d `T | NoReturn` isn’t incorrect, it’s just redundant, as it’s equivalent to `T`. You can replace `None | NoReturn` with `None` on line 14 (leaving as-is the `NoReturn` on line 10, which is not in a union), and you’ll get all the same results ([mypy playground](https://mypy-play.net/?mypy=latest&python=3.12&flags=strict%2Cwarn-unreachable&gist=33b75def0e92116f20d099eb42ab83df)).

---

_Comment by @SRv6d on 2024-04-03 14:04_

@andersk Thanks for clarifying, I falsely assumed that the return type of the function implementation had to be a union containing all the overloaded return types.   

---
