```yaml
number: 15997
title: "`--locked` should only fail if the lockfile actually changes"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-09-22T21:52:21Z
updated_at: 2025-09-23T11:25:14Z
url: https://github.com/astral-sh/uv/issues/15997
synced_at: 2026-01-12T16:02:21Z
```

# `--locked` should only fail if the lockfile actually changes

---

_@charliermarsh_

In https://github.com/astral-sh/uv/pull/15994 (for example), the lockfile doesn't actually change at all, but `--locked` fails. We need to make the lockfiles comparable.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-22 21:52_

---

_Label `bug` added by @charliermarsh on 2025-09-22 21:52_

---

_Closed by @charliermarsh on 2025-09-23 11:25_

---
