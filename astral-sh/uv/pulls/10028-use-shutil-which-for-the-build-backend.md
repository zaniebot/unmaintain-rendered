```yaml
number: 10028
title: "Use `shutil.which` for the build backend"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/which-uv
created_at: 2024-12-19T14:08:19Z
updated_at: 2024-12-20T09:15:26Z
url: https://github.com/astral-sh/uv/pull/10028
synced_at: 2026-01-12T16:09:05Z
```

# Use `shutil.which` for the build backend

---

_@konstin_

From PEP 517:

> All command-line scripts provided by the build-required packages must be present in the build environment’s PATH. For example, if a project declares a build-requirement on flit, then the following must work as a mechanism for running the flit command-line tool:
>
> ```python
> import subprocess
> import shutil
> subprocess.check_call([shutil.which("flit"), ...])
> ```

Fixes #9991

---

_Label `bug` added by @konstin on 2024-12-19 14:08_

---

_Label `preview` added by @konstin on 2024-12-19 14:08_

---

_Review requested from @charliermarsh by @konstin on 2024-12-19 14:08_

---

_@cthoyt reviewed on 2024-12-19 15:01_

---

_Review comment by @cthoyt on `python/uv/_build_backend.py`:35 on 2024-12-19 15:01_

if for some reason, uv isn't (properly) installed or can't be found on the path, `shutil.which` returns `None`. The following can guard against this and give a bit of feedback:

```suggestion
    uv_bin = shutil.which("uv")
    if uv_bin is None:
        raise RuntimeError("uv was not properly installed")
```

this should also address the type checking error in CI

---

_@konstin reviewed on 2024-12-19 15:27_

---

_Review comment by @konstin on `python/uv/_build_backend.py`:35 on 2024-12-19 15:27_

Good idea

---

_@charliermarsh approved on 2024-12-19 17:15_

---

_Merged by @konstin on 2024-12-20 09:15_

---

_Closed by @konstin on 2024-12-20 09:15_

---

_Branch deleted on 2024-12-20 09:15_

---
