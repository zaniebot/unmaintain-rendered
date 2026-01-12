```yaml
number: 5319
title: "Avoid redundant members update in `uv init`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
base: charlie/exclude
head: charlie/member
created_at: 2024-07-23T00:13:40Z
updated_at: 2024-07-23T00:15:36Z
url: https://github.com/astral-sh/uv/pull/5319
synced_at: 2026-01-12T16:06:45Z
```

# Avoid redundant members update in `uv init`

---

_@charliermarsh_

## Summary

If the path is already covered by `members`, we don't need to update it.

Closes https://github.com/astral-sh/uv/issues/5320.


---

_Renamed from "Avoid redundant members update in uv init" to "Avoid redundant members update in `uv init`" by @charliermarsh on 2024-07-23 00:13_

---

_Label `bug` added by @charliermarsh on 2024-07-23 00:13_

---

_Label `preview` added by @charliermarsh on 2024-07-23 00:13_

---

_Closed by @charliermarsh on 2024-07-23 00:15_

---
