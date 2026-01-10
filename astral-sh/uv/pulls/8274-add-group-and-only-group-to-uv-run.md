```yaml
number: 8274
title: "Add `--group` and `--only-group` to `uv run`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: tracking/735
head: zb/735-run-group
created_at: 2024-10-16T22:54:17Z
updated_at: 2024-10-18T17:06:26Z
url: https://github.com/astral-sh/uv/pull/8274
synced_at: 2026-01-10T12:54:06Z
```

# Add `--group` and `--only-group` to `uv run`

---

_Pull request opened by @zanieb on 2024-10-16 22:54_

Similar to #8110 

Part of #8090 


---

_Comment by @zanieb on 2024-10-16 23:00_

Annoyingly, we keep including the `dev` group by default even when it's not explicitly requested which messes with the warnings. Will address that separately.

---

_Marked ready for review by @zanieb on 2024-10-17 22:30_

---

_@charliermarsh approved on 2024-10-18 14:59_

---

_Merged by @zanieb on 2024-10-18 17:06_

---

_Closed by @zanieb on 2024-10-18 17:06_

---

_Branch deleted on 2024-10-18 17:06_

---
