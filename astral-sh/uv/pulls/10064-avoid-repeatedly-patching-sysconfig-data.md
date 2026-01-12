```yaml
number: 10064
title: "Avoid repeatedly patching `sysconfig` data"
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/ensure
created_at: 2024-12-20T18:21:04Z
updated_at: 2024-12-20T18:56:29Z
url: https://github.com/astral-sh/uv/pull/10064
synced_at: 2026-01-12T16:09:06Z
```

# Avoid repeatedly patching `sysconfig` data

---

_@charliermarsh_

## Summary

If we've already patched `_sysconfigdata_`, we can skip patching it again. (This is debatable; we could also just leave this.)

Closes https://github.com/astral-sh/uv/pull/10063.


---

_Label `bug` added by @charliermarsh on 2024-12-20 18:21_

---

_Marked ready for review by @charliermarsh on 2024-12-20 18:21_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-20 18:21_

---

_Comment by @charliermarsh on 2024-12-20 18:53_

I sorta defer to you @zanieb on whether you want repeated `uv python install` to potentially re-patch sysconfig.

---

_Comment by @zanieb on 2024-12-20 18:55_

What if we change what the patch includes? I think it's sort of fine as-is.

---

_Comment by @charliermarsh on 2024-12-20 18:56_

Ok, np.

---

_Closed by @charliermarsh on 2024-12-20 18:56_

---

_Branch deleted on 2024-12-20 18:56_

---
