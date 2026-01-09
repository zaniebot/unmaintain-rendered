---
number: 2893
title: SLF001 rejects acceptable private and protected access
type: issue
state: closed
author: sanmai-NL
labels:
  - bug
assignees: []
created_at: 2023-02-14T14:47:20Z
updated_at: 2023-02-15T16:52:07Z
url: https://github.com/astral-sh/ruff/issues/2893
synced_at: 2026-01-07T13:12:14-06:00
---

# SLF001 rejects acceptable private and protected access

---

_Issue opened by @sanmai-NL on 2023-02-14 14:47_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

In the following case, a static method is called from within the class definition. 
That should be allowed, and from subclasses too.

Calls to private methods should also be allowed from within the class definition.

```python
class TestSLF001:
    @staticmethod
    def _protected() -> None:
        pass

    def public() -> None:
        TestSLF001._protected()
```

```sh
ruff check --select ALL --ignore D103,TCH003,D testslf001.py
```

ruff 0.0.245

`pyproject.toml` snippet:

```toml
[tool.ruff]
select = ["ALL"]
line-length = 120
target-version = "py311"
```


---

_Renamed from "SLF001 does not discern protected from private members" to "SLF001 rejects acceptable private and protected access" by @sanmai-NL on 2023-02-14 14:51_

---

_Label `bug` added by @charliermarsh on 2023-02-14 16:03_

---

_Comment by @charliermarsh on 2023-02-14 16:04_

👍 Yeah we should accept this. (It currently accepts `self._private` and `cls._private`, if it's helpful in the interim.)

---

_Comment by @sanmai-NL on 2023-02-15 05:25_

`cls` not being the first param of a class method warrants a linting warning in its own right, indeed. And the same for `self` and instance methods.

---

_Referenced in [astral-sh/ruff#2929](../../astral-sh/ruff/pulls/2929.md) on 2023-02-15 16:47_

---

_Closed by @charliermarsh on 2023-02-15 16:52_

---
