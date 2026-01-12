```yaml
number: 2169
title: Respect local freshness when auditing installed environment
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/timestmap
created_at: 2024-03-04T19:11:21Z
updated_at: 2024-03-04T19:40:53Z
url: https://github.com/astral-sh/uv/pull/2169
synced_at: 2026-01-12T16:04:53Z
```

# Respect local freshness when auditing installed environment

---

_@charliermarsh_

## Summary

Ensures that local dependencies function similarly to editables, in that if they're `uv pip install`ed, we invalidate them.

Closes https://github.com/astral-sh/uv/issues/1651.


---

_Label `bug` added by @charliermarsh on 2024-03-04 19:11_

---

_Marked ready for review by @charliermarsh on 2024-03-04 19:11_

---

_Merged by @charliermarsh on 2024-03-04 19:40_

---

_Closed by @charliermarsh on 2024-03-04 19:40_

---

_Branch deleted on 2024-03-04 19:40_

---
