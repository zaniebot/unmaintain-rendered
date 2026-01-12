```yaml
number: 6701
title: "PLW1641/`eq-without-hash` should not trigger when explicitly setting `__hash__ = None`"
type: issue
state: closed
author: bluetech
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-08-20T10:16:12Z
updated_at: 2023-08-21T23:51:23Z
url: https://github.com/astral-sh/ruff/issues/6701
synced_at: 2026-01-12T15:54:46Z
```

# PLW1641/`eq-without-hash` should not trigger when explicitly setting `__hash__ = None`

---

_@bluetech_

Given

```py
class MyClass:
    def __eq__(self, other):
        return True

    __hash__ = None
```

Running `ruff --isolated --select PLW1641` with Ruff 0.0.285:

```
x.py:1:7: PLW1641 Object does not implement `__hash__` method
```

While the purpose of `PLW1641` is exactly to check for when `__eq__` is defined and `__hash__` is `None` (the default when `__eq__` is defined but `__hash__` is not), I think that if the code explicitly sets `__hash__ = None` then the lint should not trigger since the programmer explicitly asks for it.

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:


* (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `bug` added by @charliermarsh on 2023-08-20 14:01_

---

_Label `accepted` added by @charliermarsh on 2023-08-20 14:01_

---

_Closed by @charliermarsh on 2023-08-21 23:51_

---
