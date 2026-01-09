---
number: 13992
title: "Improve error message on \"failed to spawn\" where the Python interpreter is missing"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - help wanted
  - error messages
assignees: []
created_at: 2025-06-12T13:33:57Z
updated_at: 2025-12-08T12:22:28Z
url: https://github.com/astral-sh/uv/issues/13992
synced_at: 2026-01-07T13:12:18-06:00
---

# Improve error message on "failed to spawn" where the Python interpreter is missing

---

_Issue opened by @zanieb on 2025-06-12 13:33_

### Summary

e.g., as reported in https://github.com/astral-sh/uv/issues/13196

The error message when the Python interpreter cannot be found by an entry point is

```
error: Failed to spawn: `jupyter` Caused by: No such file or directory (os error 2)
```

We can read the entry point and provide a better error message in this case.

Related #13989 

### Example


```
error: Failed to spawn: `jupyter` Caused by: No such file or directory (os error 2)

hint: `jupyter` uses a Python interpreter at `path` which no longer exists
```

---

_Label `enhancement` added by @zanieb on 2025-06-12 13:33_

---

_Label `error messages` added by @zanieb on 2025-06-12 13:38_

---

_Label `help wanted` added by @zanieb on 2025-06-12 13:38_

---

_Referenced in [astral-sh/uv#13196](../../astral-sh/uv/issues/13196.md) on 2025-06-12 15:13_

---

_Comment by @juhaszp95 on 2025-06-12 15:44_

Just a thought: I could imagine a 'what to do' part added to the hint

---

_Comment by @zanieb on 2025-06-12 15:49_

👍 definitely

---

_Comment by @mahiro72 on 2025-12-07 12:12_

Hi @zanieb , I'd like to work on this issue.

---

_Comment by @zanieb on 2025-12-08 12:22_

Go for it.

---
