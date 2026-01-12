```yaml
number: 12136
title: "`uvx --from <path>` does not respect changes in subsequent invocations"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
assignees: []
created_at: 2025-03-12T14:15:30Z
updated_at: 2025-03-12T14:47:04Z
url: https://github.com/astral-sh/uv/issues/12136
synced_at: 2026-01-12T16:00:56Z
```

# `uvx --from <path>` does not respect changes in subsequent invocations

---

_@zanieb_

The environment is cached and not invalidated and we don't use an editable install by default.

The solution is `--with-editable <path>` which is quite verbose.

Perhaps we should just use an editable install by default here? Alternatively, we should not cache a local path?

---

_Label `needs-decision` added by @zanieb on 2025-03-12 14:47_

---
