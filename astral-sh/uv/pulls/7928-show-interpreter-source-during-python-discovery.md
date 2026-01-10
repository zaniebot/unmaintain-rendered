```yaml
number: 7928
title: Show interpreter source during Python discovery query errors
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/python-discover-err
created_at: 2024-10-04T16:08:19Z
updated_at: 2024-10-07T17:57:20Z
url: https://github.com/astral-sh/uv/pull/7928
synced_at: 2026-01-10T12:53:59Z
```

# Show interpreter source during Python discovery query errors

---

_Pull request opened by @zanieb on 2024-10-04 16:08_

Closes https://github.com/astral-sh/uv/issues/4154

e.g.

```
❯ UV_PYTHON=/dev/null cargo run -q -- pip install anyio
error: Failed to inspect Python interpreter from provided path at `/dev/null` 
  Caused by: Failed to query Python interpreter at `/dev/null`
  Caused by: Permission denied (os error 13)
  
❯ VIRTUAL_ENV=/dev/null cargo run -q -- pip install anyio
error: Failed to inspect Python interpreter from active virtual environment at `/dev/null/bin/python3` 
  Caused by: Failed to query Python interpreter
  Caused by: failed to canonicalize path `/dev/null/bin/python3`
  Caused by: Not a directory (os error 20)
```

---

_Label `error messages` added by @zanieb on 2024-10-04 16:08_

---

_Merged by @zanieb on 2024-10-07 17:57_

---

_Closed by @zanieb on 2024-10-07 17:57_

---

_Branch deleted on 2024-10-07 17:57_

---
