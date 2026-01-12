```yaml
number: 3387
title: PT016 recommends deprecated kwarg.
type: issue
state: closed
author: tobiasraabe
labels:
  - bug
assignees: []
created_at: 2023-03-07T15:43:00Z
updated_at: 2023-06-13T00:54:08Z
url: https://github.com/astral-sh/ruff/issues/3387
synced_at: 2026-01-12T15:54:43Z
```

# PT016 recommends deprecated kwarg.

---

_@tobiasraabe_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

- ruff version: 0.0.254

Hi!

Thanks for the amazing application!

Rule PT016 suggests adding a reason when using `pytest.fail()`, but when using a keyword argument it requires the deprecated `msg` keyword instead of the new `reason` kwarg introduced in v7 (https://docs.pytest.org/en/7.1.x/reference/reference.html#pytest.fail).

```python
pytest.fail("reason")  # OK
pytest.fail(msg="reason")  # OK
pytest.fail(reason="reason")  # Error PT016
```

The current behavior is the same as flake8-pytest-style (https://github.com/m-burst/flake8-pytest-style/blob/master/docs/rules/PT016.md), but I do not see a reason to stay with deprecated behavior.

Cheers!

---

_Comment by @charliermarsh on 2023-03-07 22:20_

Oh interesting! I guess the risk here is that (at least right now) we don't know your `pytest` version, so if you're on a version older than 7, using `reason` might actually be a breaking change.

---

_Label `bug` added by @charliermarsh on 2023-03-07 22:20_

---

_Comment by @charliermarsh on 2023-03-07 23:55_

We could make the `pytest` major version configurable.

---

_Comment by @charliermarsh on 2023-03-07 23:56_

@edgarrmondragon - do you know if Pytest v7 is widely adopted? Are a lot of projects on older versions?

---

_Comment by @edgarrmondragon on 2023-03-08 00:40_

@charliermarsh Did some quick numbers from the PyPI download stats starting from 2023-01-01 and by that it seems pytest 7 is indeed widely adopted 🙂 

|   major | downloads   |
|--------:|:------------|
|       2 | 288,744     |
|       3 | 2,076,701   |
|       4 | 5,356,972   |
|       5 | 8,021,611   |
|       6 | 25,819,778  |
|       7 | 80,133,528  |


---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-12 23:51_

---

_Closed by @charliermarsh on 2023-06-13 00:54_

---
