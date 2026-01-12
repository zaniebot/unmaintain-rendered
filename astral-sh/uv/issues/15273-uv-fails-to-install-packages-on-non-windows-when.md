```yaml
number: 15273
title: uv fails to install packages on non-Windows when win32-specific dependencies are missing from private index
type: issue
state: closed
author: bombedhair
labels:
  - question
assignees: []
created_at: 2025-08-14T10:36:34Z
updated_at: 2025-08-14T10:47:30Z
url: https://github.com/astral-sh/uv/issues/15273
synced_at: 2026-01-12T16:02:07Z
```

# uv fails to install packages on non-Windows when win32-specific dependencies are missing from private index

---

_@bombedhair_

### Summary

I have set up a private index with Nexus Repository and published only the packages and their dependencies necessary for my project development in an Ubuntu environment.
However, I am encountering an error during the package installation process, as it cannot find a dependency marked as `sys_platform == win32`. For example, when I try `uv add uvicorn[standard]`, the following error occurs for colorama, which is listed as `Requires-Dist: colorama>=0.4; (sys_platform == 'win32') and extra == 'standard'` in metadata.
```
Creating virtual environment at: .venv
  × No solution found when resolving dependencies:
  ╰─▶ Because only colorama{sys_platform == 'win32'}<0.4 is available and uvicorn[standard]==0.35.0 depends on
      colorama{sys_platform == 'win32'}>=0.4, we can conclude that uvicorn[standard]==0.35.0 cannot be used.
      And because only uvicorn[standard]==0.35.0 is available and your project depends on
      uvicorn[standard]==0.35.0, we can conclude that your project's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `—frozen` flag to skip
        locking and syncing
```
I would like to know if there is a proper solution other than publishing the unused dependency to the private index or using the `--frozen` flag



### Platform

Ubuntu 24.04

### Version

0.8.10

### Python version

Python 3.11.13

---

_Label `bug` added by @bombedhair on 2025-08-14 10:36_

---

_Comment by @charliermarsh on 2025-08-14 10:38_

I would suggest: https://docs.astral.sh/uv/concepts/resolution/#limited-resolution-environments

In short, you could mark Windows as unsupported:
```toml
[tool.uv]
environments = [
    "sys_platform != 'win32'",
]
```

---

_Comment by @bombedhair on 2025-08-14 10:44_

Ah! Thank you for providing a clear solution.
Questions label would have been a better fit for this issue. My apologies for making inconveniences; I was in bit hurry

---

_Closed by @bombedhair on 2025-08-14 10:44_

---

_Comment by @charliermarsh on 2025-08-14 10:47_

All good, don't worry about it :)

---

_Label `bug` removed by @charliermarsh on 2025-08-14 10:47_

---

_Label `question` added by @charliermarsh on 2025-08-14 10:47_

---
