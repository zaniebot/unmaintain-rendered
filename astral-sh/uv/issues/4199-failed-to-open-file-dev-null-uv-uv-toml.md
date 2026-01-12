```yaml
number: 4199
title: "failed to open file `/dev/null/uv/uv.toml`"
type: issue
state: closed
author: misery
labels:
  - bug
assignees: []
created_at: 2024-06-10T14:01:38Z
updated_at: 2024-06-10T17:36:31Z
url: https://github.com/astral-sh/uv/issues/4199
synced_at: 2026-01-12T15:58:48Z
```

# failed to open file `/dev/null/uv/uv.toml`

---

_@misery_

I tried uv in pre-receive hook in gitlab. Seems gitlab misses something here. But it would be nice if uv won't fail anymore.

Tried 0.2.9
```
uv -v venv $VIRTUAL_ENV
 error: failed to open file `/dev/null/uv/uv.toml`
   Caused by: Not a directory (os error 20)
```


---

_Label `bug` added by @zanieb on 2024-06-10 14:03_

---

_Comment by @zanieb on 2024-06-10 14:03_

Thanks for the report!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-10 15:49_

---

_Closed by @charliermarsh on 2024-06-10 17:36_

---
