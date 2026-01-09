---
number: 1796
title: "Ignore `__exit__` for ARG002"
type: issue
state: closed
author: ofek
labels: []
assignees: []
created_at: 2023-01-11T23:14:12Z
updated_at: 2023-01-12T00:02:22Z
url: https://github.com/astral-sh/ruff/issues/1796
synced_at: 2026-01-07T13:12:14-06:00
---

# Ignore `__exit__` for ARG002

---

_Issue opened by @ofek on 2023-01-11 23:14_

This method is implemented for classes to support being used as a context manager and it would be nice to add an exception for this method because the arguments are often unused. For example, this is what I've been doing:

```python
    def __exit__(
        self,
        exc_type: type[BaseException] | None,  # noqa: ARG002
        exc_value: BaseException | None,  # noqa: ARG002
        traceback: TracebackType | None,  # noqa: ARG002
    ) -> None:
        # cleanup logic
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-11 23:14_

---

_Comment by @charliermarsh on 2023-01-11 23:15_

Sounds good.

---

_Comment by @charliermarsh on 2023-01-11 23:34_

Perhaps we should omit all "magic" methods...?

---

_Referenced in [astral-sh/ruff#1801](../../astral-sh/ruff/pulls/1801.md) on 2023-01-12 00:02_

---

_Closed by @charliermarsh on 2023-01-12 00:02_

---

_Closed by @charliermarsh on 2023-01-12 00:02_

---

_Referenced in [astral-sh/ruff#21904](../../astral-sh/ruff/issues/21904.md) on 2025-12-10 20:18_

---
