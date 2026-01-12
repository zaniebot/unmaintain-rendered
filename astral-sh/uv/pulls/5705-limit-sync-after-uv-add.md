```yaml
number: 5705
title: "Limit sync after `uv add`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/sync-add
created_at: 2024-08-01T19:02:25Z
updated_at: 2024-08-01T20:22:40Z
url: https://github.com/astral-sh/uv/pull/5705
synced_at: 2026-01-12T16:06:58Z
```

# Limit sync after `uv add`

---

_@charliermarsh_

## Summary

I think it's reasonable to only sync the affected group, e.g., `uv add` on its own should not require syncing all extras.

Closes https://github.com/astral-sh/uv/issues/4418.


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-01 19:02_

---

_Label `cli` added by @charliermarsh on 2024-08-01 19:02_

---

_Label `preview` added by @charliermarsh on 2024-08-01 19:02_

---

_@ibraheemdev approved on 2024-08-01 19:10_

---

_Merged by @zanieb on 2024-08-01 20:22_

---

_Closed by @zanieb on 2024-08-01 20:22_

---

_Branch deleted on 2024-08-01 20:22_

---
