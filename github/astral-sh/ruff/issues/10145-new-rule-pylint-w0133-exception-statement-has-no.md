---
number: 10145
title: "[new rule] pylint W0133: Exception statement has no effect"
type: issue
state: closed
author: Taragolis
labels:
  - rule
assignees: []
created_at: 2024-02-27T16:17:12Z
updated_at: 2024-03-11T17:39:06Z
url: https://github.com/astral-sh/ruff/issues/10145
synced_at: 2026-01-07T13:12:15-06:00
---

# [new rule] pylint W0133: Exception statement has no effect

---

_Issue opened by @Taragolis on 2024-02-27 16:17_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

This rule already requested into the https://github.com/astral-sh/ruff/issues/8674, however suggested replacement `B018` can not handle all the cases.

For example this snippet do not violate any of `B018` checks: https://play.ruff.rs/bef7e65b-09c6-4e8f-84e9-48a75be4e0b9

```python
# w0133.py
from __future__ import annotations


def some_func(b: str, *, a: str | None = None, raise_an_error=True):
    if a:
        msg = "`a` is deprecated, consider to use `b` instead."
        if raise_an_error:
            msg = "Use `b` instead."
            ValueError(msg)
        DeprecationWarning(msg)
        b = a
    return b


class AwesomeClass:
    def __init__(self, b: str | None = None, **kwargs):
        if a := kwargs.pop("a", None):
            DeprecationWarning("`a` is deprecated, consider to use `b` instead.")
            if b and a != b:
                msg = f"Unable to resolve ambiguous values b={b!r}, a={a!r}."
                ValueError(msg)
            b = a
        self.b = b

    def abstract(self):
        NotImplementedError("abstract should be implemented")


ValueError("Foo")
DeprecationWarning("Bar")


if __name__ == "__main__":
    ValueError("Spam")
    DeprecationWarning("Egg")
```

In the other hand  `pylint` rule [`W0133`](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/pointless-exception-statement.html) catch all useless exceptions and warnings

```console
❯ pipx run pylint --disable=all --enable="W0133" w0133.py
************* Module w0133
w0133.py:10:12: W0133: Exception statement has no effect (pointless-exception-statement)
w0133.py:11:8: W0133: Exception statement has no effect (pointless-exception-statement)
w0133.py:19:12: W0133: Exception statement has no effect (pointless-exception-statement)
w0133.py:22:16: W0133: Exception statement has no effect (pointless-exception-statement)
w0133.py:27:8: W0133: Exception statement has no effect (pointless-exception-statement)
w0133.py:30:0: W0133: Exception statement has no effect (pointless-exception-statement)
w0133.py:31:0: W0133: Exception statement has no effect (pointless-exception-statement)
w0133.py:35:4: W0133: Exception statement has no effect (pointless-exception-statement)
w0133.py:36:4: W0133: Exception statement has no effect (pointless-exception-statement)
```

It would be nice if it possible to have this rule as part of the `ruff`, this might help to found such a mistakes in codebase of pretty big projects.

---

_Referenced in [apache/airflow#37722](../../apache/airflow/pulls/37722.md) on 2024-02-27 16:21_

---

_Label `rule` added by @AlexWaygood on 2024-02-27 21:35_

---

_Comment by @mikeleppane on 2024-02-28 07:37_

I could take a look at this. Let’s see, if this can be reasonably extended to the B018. Otherwise, this may require a separate rule?

---

_Comment by @Taragolis on 2024-02-28 09:12_

If you ask me then I do not not have any preferences, so it could be either separate rule something like `PLW0133` or extension of `B018`.


---

_Referenced in [astral-sh/ruff#10176](../../astral-sh/ruff/pulls/10176.md) on 2024-02-29 17:10_

---

_Comment by @zanieb on 2024-03-11 17:39_

Done in https://github.com/astral-sh/ruff/pull/10176

---

_Closed by @zanieb on 2024-03-11 17:39_

---
