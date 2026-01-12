```yaml
number: 11209
title: Set base executable when returning virtual environment
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/base-exec
created_at: 2025-02-04T01:28:29Z
updated_at: 2025-02-04T22:32:48Z
url: https://github.com/astral-sh/uv/pull/11209
synced_at: 2026-01-12T16:09:44Z
```

# Set base executable when returning virtual environment

---

_@charliermarsh_

## Summary

I'm not sure that this has much of an effect in practice, but currently, when we return a virtual environment, the `sys_base_executable ` of the parent ends up being retained as `sys_base_executable` of the created environment. But these can be, like, subtly different? If you have a symlink to a Python, then for the symlink, `sys_base_executable` will be equal to `sys_executable`. But when you create a virtual environment for that interpreter, we'll set `home` to the resolved symlink, and so `sys_base_executable` will be the resolved symlink too, in general. Anyway, this means that we should now have a consistent value between (1) returning `Virtualenv` from the creation routine and (2) querying the created interpreter.


---

_Label `bug` added by @charliermarsh on 2025-02-04 01:28_

---

_Review requested from @zanieb by @zanieb on 2025-02-04 02:19_

---

_@zanieb approved on 2025-02-04 22:06_

---

_Merged by @charliermarsh on 2025-02-04 22:32_

---

_Closed by @charliermarsh on 2025-02-04 22:32_

---

_Branch deleted on 2025-02-04 22:32_

---
