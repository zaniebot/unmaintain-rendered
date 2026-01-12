```yaml
number: 14641
title: "`check system | aarch64 python3.13 on windows aarch64` flakes with pylint not installed"
type: issue
state: closed
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-07-15T22:55:14Z
updated_at: 2025-07-16T14:10:00Z
url: https://github.com/astral-sh/uv/issues/14641
synced_at: 2026-01-12T16:01:53Z
```

# `check system | aarch64 python3.13 on windows aarch64` flakes with pylint not installed

---

_@zanieb_

```
Prepared 8 packages in 396ms
Installed 8 packages in 133ms
 + astroid==3.3.11
 + colorama==0.4.6
 + dill==0.4.0
 + isort==6.0.1
 + mccabe==0.7.0
 + platformdirs==4.3.8
 + pylint==3.3.7
 + tomlkit==0.13.3
DEBUG Released lock at `C:\Users\RUNNER~1\AppData\Local\Temp\uv-31d99a0eddb8f452.lock`
INFO: Checking that `pylint` is installed.
WARNING: Package(s) not found: pylint
Traceback (most recent call last):
  File "C:\a\uv\uv\scripts\check_system_python.py", line 91, in <module>
    raise Exception("The package `pylint` isn't installed (but should be).")
Exception: The package `pylint` isn't installed (but should be).
```

e.g. https://github.com/astral-sh/uv/actions/runs/16305987238/job/46052178024?pr=14640#logs

---

_Label `ci-flake` added by @zanieb on 2025-07-15 22:55_

---

_Comment by @zanieb on 2025-07-16 14:10_

Closed by #14655 

---

_Closed by @zanieb on 2025-07-16 14:10_

---
