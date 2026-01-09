---
number: 4285
title: B027 false positive with abc.abstractclassmethod
type: issue
state: closed
author: Skylion007
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2023-05-08T14:35:04Z
updated_at: 2023-05-09T01:54:03Z
url: https://github.com/astral-sh/ruff/issues/4285
synced_at: 2026-01-07T13:12:14-06:00
---

# B027 false positive with abc.abstractclassmethod

---

_Issue opened by @Skylion007 on 2023-05-08 14:35_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Adding an abstractmethod decorator does not make sense if an abstractclassmethod decorator is used instead:
When doing an automated fixes to the PyTorch codebase I ran into an issue in the wild:

```
class Foo(metaclass=abc.ABCMeta):
    @abc.abstractclassmethod
    def read(self): 
        pass
```
should not be flagged for instance (but currently is).  This should be an easy exclusion to add to the rule and a good first issue.


---

_Label `bug` added by @charliermarsh on 2023-05-08 17:57_

---

_Label `good first issue` added by @charliermarsh on 2023-05-08 17:57_

---

_Label `help wanted` added by @charliermarsh on 2023-05-08 17:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-09 01:42_

---

_Comment by @charliermarsh on 2023-05-09 01:42_

Just gonna fix this real quick.

---

_Referenced in [astral-sh/ruff#4298](../../astral-sh/ruff/pulls/4298.md) on 2023-05-09 01:48_

---

_Closed by @charliermarsh on 2023-05-09 01:54_

---
