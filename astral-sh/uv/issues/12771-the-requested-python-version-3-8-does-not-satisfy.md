```yaml
number: 12771
title: "the requested Python version (>=3.8) does not satisfy when use add"
type: issue
state: closed
author: bigblackhat-cpu
labels:
  - question
assignees: []
created_at: 2025-04-09T03:36:30Z
updated_at: 2025-07-16T13:48:26Z
url: https://github.com/astral-sh/uv/issues/12771
synced_at: 2026-01-12T16:01:12Z
```

# the requested Python version (>=3.8) does not satisfy when use add

---

_@bigblackhat-cpu_

### Question

```
Jeremy@x2002064:~/PROJECT/UV_Project/weather$ uv python pin 3.10
Updated `.python-version` from `3.8` -> `3.10`
Jeremy@x2002064:~/PROJECT/UV_Project/weather$ uv add "mcp[cli]" httpx
  × No solution found when resolving dependencies for split
  │ (python_full_version >= '3.8' and python_full_version < '3.10'):
  ╰─▶ Because the requested Python version (>=3.8) does not satisfy
      Python>=3.10 and your project depends on mcp[cli], we can conclude that
      your project's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution,
        provide the `--frozen` flag to skip locking and syncing.
Jeremy@x2002064:~/PROJECT/UV_Project/weather$
``` 
I have already used Python 3.10, why is it still the requested Python version (>=3.8) does not satisfy?

### Platform

liunx

### Version

_No response_

---

_Label `question` added by @bigblackhat-cpu on 2025-04-09 03:36_

---

_Comment by @charliermarsh on 2025-04-10 14:10_

I think you want to edit your `requires-python`? That determines the minimum-supported Python version for your project. We need to be able to resolve for all supported versions.

---

_Closed by @charliermarsh on 2025-07-16 13:48_

---
