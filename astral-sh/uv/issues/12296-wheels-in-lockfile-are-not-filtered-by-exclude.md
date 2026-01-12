```yaml
number: 12296
title: "Wheels in lockfile are not filtered by `exclude-newer`"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-03-18T18:06:37Z
updated_at: 2025-03-22T16:27:12Z
url: https://github.com/astral-sh/uv/issues/12296
synced_at: 2026-01-12T16:01:00Z
```

# Wheels in lockfile are not filtered by `exclude-newer`

---

_@zanieb_

See https://github.com/astral-sh/uv/pull/12294 for an example snapshot change

---

_Assigned to @charliermarsh by @zanieb on 2025-03-18 18:06_

---

_Label `bug` added by @charliermarsh on 2025-03-18 18:39_

---

_Comment by @charliermarsh on 2025-03-18 18:40_

Note: `pip compile` is also affected in that it will include hashes generated after `--exclude-newer` if you pass `--generate-hashes`.

---

_Closed by @charliermarsh on 2025-03-22 16:27_

---

_Closed by @charliermarsh on 2025-03-22 16:27_

---
