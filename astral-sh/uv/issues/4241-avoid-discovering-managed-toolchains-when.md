```yaml
number: 4241
title: "Avoid discovering managed toolchains when mutating environment with `--system` flag"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2024-06-11T17:51:35Z
updated_at: 2025-02-18T15:45:46Z
url: https://github.com/astral-sh/uv/issues/4241
synced_at: 2026-01-12T15:58:49Z
```

# Avoid discovering managed toolchains when mutating environment with `--system` flag

---

_@zanieb_

Currently, `uv pip install --system` could write to a managed toolchain.

---

_Label `bug` added by @zanieb on 2024-06-11 17:51_

---

_Assigned to @zanieb by @zanieb on 2024-06-11 17:51_

---

_Label `preview` added by @zanieb on 2024-06-11 17:51_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Comment by @zanieb on 2025-02-18 15:45_

We write an `EXTERNALLY-MANAGED` marker here.

---

_Closed by @zanieb on 2025-02-18 15:45_

---
