```yaml
number: 5928
title: Resolve from lockfile cannot detect yanked versions
type: issue
state: closed
author: ibraheemdev
labels:
  - preview
  - lock
assignees: []
created_at: 2024-08-08T20:21:18Z
updated_at: 2024-08-19T17:52:53Z
url: https://github.com/astral-sh/uv/issues/5928
synced_at: 2026-01-12T15:59:00Z
```

# Resolve from lockfile cannot detect yanked versions

---

_@ibraheemdev_

When resolving from the lockfile we don't query the registry and so cannot detect yanked versions. We should document this or add an explicit check for yanked versions even when resolving from the lockfile.

---

_Label `preview` added by @ibraheemdev on 2024-08-08 20:21_

---

_Label `lock` added by @ibraheemdev on 2024-08-08 20:21_

---

_Assigned to @zanieb by @charliermarsh on 2024-08-09 14:19_

---

_Comment by @zanieb on 2024-08-09 14:44_

I will document this caveat, it's probably infeasible to support warning here at runtime.

---

_Comment by @zanieb on 2024-08-09 17:15_

Is this relevant for `uv run` and `uv sync` or more?

---

_Closed by @zanieb on 2024-08-19 17:52_

---

_Closed by @zanieb on 2024-08-19 17:52_

---
