```yaml
number: 6343
title: "Avoid initializing `ResolverReporter` for no-op cases"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - help wanted
  - cli
assignees: []
created_at: 2024-08-21T15:47:20Z
updated_at: 2024-09-15T18:18:29Z
url: https://github.com/astral-sh/uv/issues/6343
synced_at: 2026-01-10T04:45:09Z
```

# Avoid initializing `ResolverReporter` for no-op cases

---

_Issue opened by @charliermarsh on 2024-08-21 15:47_

In some cases, initializing the `ResolverReporter` can cause the progress bar to flash.

---

_Label `bug` added by @charliermarsh on 2024-08-21 15:47_

---

_Label `cli` added by @charliermarsh on 2024-08-21 15:47_

---

_Comment by @charliermarsh on 2024-08-21 17:28_

Or add some kind of `start` API for the reporters.

---

_Label `help wanted` added by @charliermarsh on 2024-08-21 17:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-15 18:11_

---

_Closed by @charliermarsh on 2024-09-15 18:18_

---

_Closed by @charliermarsh on 2024-09-15 18:18_

---
