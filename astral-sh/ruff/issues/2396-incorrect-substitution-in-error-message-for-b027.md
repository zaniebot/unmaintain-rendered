```yaml
number: 2396
title: "Incorrect substitution in error message for `B027`"
type: issue
state: closed
author: ngnpope
labels:
  - bug
assignees: []
created_at: 2023-01-31T14:00:09Z
updated_at: 2023-01-31T17:41:24Z
url: https://github.com/astral-sh/ruff/issues/2396
synced_at: 2026-01-10T11:09:45Z
```

# Incorrect substitution in error message for `B027`

---

_Issue opened by @ngnpope on 2023-01-31 14:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->


For the following minimal reproducer:

```python
import abc

class Class(abc.ABC):
    def method(self):
        pass
```

I get the following:

```console
❯ ruff --version
ruff 0.0.238

❯ ruff --select B027 bug.py
bug.py:4:5: B027 `Class` is an empty method in an abstract base class, but has no abstract decorator
Found 1 error.
```

I'd expect either `Class.method()` or `method()` instead of `Class`.

Not sure whether `"... no abstract decorator"` would be better as `"... no abstractmethod decorator"` or `"... no @abstractmethod decorator"`?

---

_Label `bug` added by @charliermarsh on 2023-01-31 14:14_

---

_Comment by @charliermarsh on 2023-01-31 14:14_

Looks like a bug!

---

_Comment by @ngnpope on 2023-01-31 14:52_

What can I say? I'm on a roll! 😛 

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-31 17:37_

---

_Closed by @charliermarsh on 2023-01-31 17:41_

---
