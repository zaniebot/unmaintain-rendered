```yaml
number: 14608
title: Cannot install a package from a local folder
type: issue
state: closed
author: pschmutz
labels:
  - bug
assignees: []
created_at: 2025-07-14T14:56:00Z
updated_at: 2025-07-14T15:34:14Z
url: https://github.com/astral-sh/uv/issues/14608
synced_at: 2026-01-12T16:01:52Z
```

# Cannot install a package from a local folder

---

_@pschmutz_

### Summary

I have the following `pyproject.toml`

```toml
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = [
    "rl-zoo3>=2.6.0",
]

[tool.uv.sources]
rl-zoo3 = [
  { path = "../rl-baselines3-zoo" },
]
```

The rl-baselines3-zoo is cloned as follows in the parent directory
```bash
git clone --branch v2.6.0 --single-branch https://github.com/DLR-RM/rl-baselines3-zoo
```

If I try to import the module I get an error

```bash
$ uv run python -c "import rl_zoo3"
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.12`.
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'rl_zoo3'
```

If I remove the `tool.uv.sources` section and use the following `pyproject.toml` it works

```toml
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = [
    "rl-zoo3>=2.6.0",
]
```

My expectation was that I can import the locally installed package. Or that uv throws an error because a dependency cannot be installed.



### Platform

Linux 6.11.0-29-generic x86_64 GNU/Linux

### Version

uv 0.7.20

### Python version

Python 3.13.5

---

_Label `bug` added by @pschmutz on 2025-07-14 14:56_

---

_Comment by @charliermarsh on 2025-07-14 14:59_

Can you try adding `{ path = "../rl-baselines3-zoo", package = true }`?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-14 15:00_

---

_Comment by @pschmutz on 2025-07-14 15:04_

This worked! Thank you so much for the quick help :) 

---

_Comment by @charliermarsh on 2025-07-14 15:34_

No worries! We have a PR open to make this the default: https://github.com/astral-sh/uv/pull/14413. The relevant issue is https://github.com/astral-sh/uv/issues/12015 so I'll merge into that.

---

_Closed by @charliermarsh on 2025-07-14 15:34_

---
