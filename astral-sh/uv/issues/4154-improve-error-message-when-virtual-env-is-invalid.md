```yaml
number: 4154
title: "Improve error message when `VIRTUAL_ENV` is invalid"
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-06-08T00:56:37Z
updated_at: 2024-10-07T17:57:19Z
url: https://github.com/astral-sh/uv/issues/4154
synced_at: 2026-01-12T15:58:48Z
```

# Improve error message when `VIRTUAL_ENV` is invalid

---

_@zanieb_

```
❯ VIRTUAL_ENV=/dev/null uv pip install anyio
error: failed to canonicalize path `/dev/null/bin/python3`
  Caused by: Not a directory (os error 20)
```

We should explain why we're trying to canonicalize this path.

---

_Label `error messages` added by @zanieb on 2024-06-08 00:56_

---

_Assigned to @zanieb by @zanieb on 2024-06-08 00:56_

---

_Comment by @zanieb on 2024-10-04 16:02_

Now we say

```
❯ VIRTUAL_ENV=/dev/null uv pip install anyio
error: Failed to query Python interpreter
  Caused by: failed to canonicalize path `/dev/null/bin/python3`
  Caused by: Not a directory (os error 20)
```

---

_Closed by @zanieb on 2024-10-07 17:57_

---
