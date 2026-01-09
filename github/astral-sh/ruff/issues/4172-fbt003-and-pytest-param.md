---
number: 4172
title: "FBT003 and `pytest.param`"
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2023-05-01T20:11:15Z
updated_at: 2023-05-02T01:07:52Z
url: https://github.com/astral-sh/ruff/issues/4172
synced_at: 2026-01-07T13:12:14-06:00
---

# FBT003 and `pytest.param`

---

_Issue opened by @jamesbraza on 2023-05-01 20:11_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

[`pytest.param`](https://docs.pytest.org/en/6.2.x/reference.html#pytest.param) is designed for `pytest.mark.parametrize`.

```python
import pytest

@pytest.mark.parametrize(
    ("arg1", "arg2"),
    [pytest.param("spam", False), pytest.param("ham", True)],
)
def test_foo(arg1: str, arg2: bool) -> None:  # noqa: FBT001
    pass
```

Running `ruff==0.0.263` on this:

```none
a.py:5:27: FBT003 Boolean positional value in function call
a.py:5:55: FBT003 Boolean positional value in function call
```

Running `flake8==6.0.0` with `flake8-boolean-trap==1.0.0` on this, there is no output.  Somehow, `ruff` seems to have a more comprehensive `FBT003` checker.

`pytest.param` doesn't support kwargs, so one has to use boolean positionals.  Any thoughts on filtering out `FBT003` for `pytest.param`?

---

_Comment by @charliermarsh on 2023-05-02 00:55_

I don't mind filtering it out, I think we already have an allowlist for these.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-02 01:00_

---

_Referenced in [astral-sh/ruff#4176](../../astral-sh/ruff/pulls/4176.md) on 2023-05-02 01:01_

---

_Closed by @charliermarsh on 2023-05-02 01:07_

---

_Referenced in [astral-sh/ruff#10485](../../astral-sh/ruff/issues/10485.md) on 2024-03-20 14:38_

---
