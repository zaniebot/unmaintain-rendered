```yaml
number: 9761
title: Show a dedicated error for missing subdirectories
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/dir
created_at: 2024-12-10T02:37:03Z
updated_at: 2024-12-10T02:53:29Z
url: https://github.com/astral-sh/uv/pull/9761
synced_at: 2026-01-12T16:08:58Z
```

# Show a dedicated error for missing subdirectories

---

_@charliermarsh_


## Summary

On `main`, if you ask for a source but name a missing subdirectory, you just get:

```
{source} does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` are present in the directory
```

But, in reality, the directory doesn't exist at all.


---

_Label `error messages` added by @charliermarsh on 2024-12-10 02:37_

---

_Merged by @charliermarsh on 2024-12-10 02:48_

---

_Closed by @charliermarsh on 2024-12-10 02:48_

---

_Branch deleted on 2024-12-10 02:48_

---

_@zanieb approved on 2024-12-10 02:53_

Appreciate all this error message work!

---
