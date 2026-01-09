---
number: 6054
title: "Is it possible to avoid raising `PT012` if `with pytest.raises` contains a `with` statement with a single raise statement? "
type: issue
state: closed
author: harupy
labels:
  - needs-info
assignees: []
created_at: 2023-07-25T06:24:13Z
updated_at: 2023-07-26T01:43:33Z
url: https://github.com/astral-sh/ruff/issues/6054
synced_at: 2026-01-07T13:12:15-06:00
---

# Is it possible to avoid raising `PT012` if `with pytest.raises` contains a `with` statement with a single raise statement? 

---

_Issue opened by @harupy on 2023-07-25 06:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Is it possible to avoid raising `PT012` if `with pytest.raises` contains a `with` statement with a single raise statement? 

Example

```python
def test_start_run_does_not_suppress_error():
    with pytest.raises(Exception, match="foo"):
        with mlflow.start_run(...):
            raise Exception("foo")

    assert ...
```

References

- https://github.com/m-burst/flake8-pytest-style/blob/master/docs/rules/PT012.md

---

_Comment by @harupy on 2023-07-25 06:24_

I found a workaround, but I prefer the original code:

```python
def test_start_run_does_not_suppress_error():
    with pytest.raises(...), mlflow.start_run(...):
         raise Exception(...)
```


---

_Comment by @charliermarsh on 2023-07-25 14:10_

Can you say a bit more about the intent of the code? E.g. why raise within the context manager?

---

_Label `waiting-on-author` added by @charliermarsh on 2023-07-25 14:10_

---

_Comment by @zanieb on 2023-07-25 14:14_

@charliermarsh I'd use this pattern to write assertions about the handling of exceptions by the `ml_flow.start_run` context manager. The example asserts that it does not swallow the exception (or raises it with a new type).

---

_Comment by @harupy on 2023-07-25 14:29_

@charliermarsh @zanieb Thanks for the comments! 

> The example asserts that it does not swallow the exception (or raises it with a new type).

Yes, exactly.

---

_Comment by @charliermarsh on 2023-07-25 15:02_

I think it's reasonable to accept any `with` with a single-statement body.

---

_Comment by @harupy on 2023-07-25 15:30_

Should we allow this example?

```python
import pytest


def test_foo():
    with pytest.raises(AttributeError):
        with foo():
            if True:
                please()
                guess()
                which()
                line()
                throws()
```

---

_Comment by @harupy on 2023-07-25 15:38_

Can we only accept `with` with a non-compound-single-statement (e.g. `raise`) body?

---

_Comment by @charliermarsh on 2023-07-25 15:40_

Yeah that would be my preference. A `with` that contains one simple statement (no compound statements).

---

_Comment by @harupy on 2023-07-25 15:47_

Sounds good. I'll file a PR tomorrow.

---

_Comment by @charliermarsh on 2023-07-25 15:52_

Thank you @harupy :)

---

_Referenced in [astral-sh/ruff#6081](../../astral-sh/ruff/pulls/6081.md) on 2023-07-26 01:10_

---

_Closed by @charliermarsh on 2023-07-26 01:43_

---
