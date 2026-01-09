---
number: 13551
title: uv doesnt update the package version when installed with uv pip install
type: issue
state: closed
author: jzr-supove
labels:
  - question
assignees: []
created_at: 2025-05-20T10:16:02Z
updated_at: 2025-05-21T06:22:39Z
url: https://github.com/astral-sh/uv/issues/13551
synced_at: 2026-01-07T13:12:18-06:00
---

# uv doesnt update the package version when installed with uv pip install

---

_Issue opened by @jzr-supove on 2025-05-20 10:16_

### Summary

When I run `uv add package-name`, it installs an older version of the package unless I manually specify the version with `uv pip install package-name==x.x.x`. Why is that?

After updating the package version, how do I update the lock file? Shouldn’t it update automatically?
Running `uv lock` doesn’t seem to do anything, and `uv sync` reverts the package back to the old version listed in `uv.lock`.

At this point, I’m seeing more headaches than benefits compared to the standard `venv + pip + requirements.txt` workflow.

### Platform

Fedora (Linux 6.13.11-200.fc41.x86_64 x86_64 GNU/Linux)

### Version

uv 0.6.2

### Python version

Python 3.9.21

---

_Label `bug` added by @jzr-supove on 2025-05-20 10:16_

---

_Label `bug` removed by @konstin on 2025-05-20 11:32_

---

_Label `question` added by @konstin on 2025-05-20 11:32_

---

_Comment by @konstin on 2025-05-20 11:33_

If the last version of `package-name` is incompatible with the other locked dependencies, a lower, compatible version is chosen. Can you try running with `uv add --upgrade`? This will also update the lockfile. By default, the lockfile only changes when necessary, to avoid churn and unrelated things breaking due to updates.


---

_Closed by @jzr-supove on 2025-05-21 06:22_

---
