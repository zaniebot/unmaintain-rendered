```yaml
number: 15908
title: "Fix root of `uv tree` when `--package` is used with circular dependencies"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/cycle-tree
created_at: 2025-09-17T12:14:47Z
updated_at: 2025-10-27T02:01:02Z
url: https://github.com/astral-sh/uv/pull/15908
synced_at: 2026-01-10T06:36:15Z
```

# Fix root of `uv tree` when `--package` is used with circular dependencies

---

_Pull request opened by @zanieb on 2025-09-17 12:14_

Closes #15907

Best viewed with https://github.com/astral-sh/uv/pull/15908/files?diff=unified&w=1

When `--package` is used, just use those as the roots rather than calculating them. I'm not sure if there will be undesirable side-effects, but it's the naive solution.

---

_Label `bug` added by @zanieb on 2025-09-17 12:14_

---

_Marked ready for review by @zanieb on 2025-10-03 19:41_

---

_@charliermarsh approved on 2025-10-27 02:00_

---

_Merged by @charliermarsh on 2025-10-27 02:01_

---

_Closed by @charliermarsh on 2025-10-27 02:01_

---

_Branch deleted on 2025-10-27 02:01_

---
