```yaml
number: 14391
title: uv lock --check fails with up to date lock file
type: issue
state: closed
author: niberger
labels:
  - bug
  - duplicate
assignees: []
created_at: 2025-07-01T13:05:30Z
updated_at: 2025-07-02T00:45:24Z
url: https://github.com/astral-sh/uv/issues/14391
synced_at: 2026-01-12T16:01:47Z
```

# uv lock --check fails with up to date lock file

---

_@niberger_

### Summary

Minimal reproducible example `pyproject.toml`:

```
[project]
name = "uv-lock-check-bug"
version = "0.1.0"
dependencies = [
    "isaaclab[isaacsim]==2.0.2",
    "nvidia-cudnn-cu12",
    "nvidia-cuda-nvrtc-cu12"
]
requires-python = "==3.10.12"

[tool.uv.sources]
isaaclab = { index = "nvidia" }
nvidia-cudnn-cu12 = { index = "pypi" }
nvidia-cuda-nvrtc-cu12 = { index = "pypi" }

[[tool.uv.index]]
name = "nvidia"
url = "https://pypi.nvidia.com/"

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
````

With `uv` 0.7.17:
```
$ uv lock
Using CPython 3.10.12 interpreter at: /usr/bin/python3
Resolved 116 packages in 686ms
$ uv lock --check
Using CPython 3.10.12 interpreter at: /usr/bin/python3
Resolved 116 packages in 612ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
````

Note: this works fine with uv 0.7.15

### Platform

Ubuntu 20.04 amd64

### Version

uv 0.7.17

### Python version

Python 3.10.12

---

_Label `bug` added by @niberger on 2025-07-01 13:05_

---

_Comment by @zanieb on 2025-07-01 13:17_

You're encountering the bug that was fixed by https://github.com/astral-sh/uv/pull/14349 and will be released soon (probably today)

---

_Label `duplicate` added by @zanieb on 2025-07-01 13:17_

---

_Closed by @charliermarsh on 2025-07-02 00:45_

---

_Reopened by @charliermarsh on 2025-07-02 00:45_

---

_Closed by @charliermarsh on 2025-07-02 00:45_

---
