```yaml
number: 17116
title: "Accept `--torch-backend` in `[tool.uv]`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/tool-uv
created_at: 2025-12-13T02:46:06Z
updated_at: 2025-12-19T14:40:59Z
url: https://github.com/astral-sh/uv/pull/17116
synced_at: 2026-01-10T05:49:14Z
```

# Accept `--torch-backend` in `[tool.uv]`

---

_Pull request opened by @charliermarsh on 2025-12-13 02:46_

## Summary

I'd like to add `--torch-backend` to `uv tool`, so this PR lifts the setting out of `[tool.uv.pip]`. Like other settings, if it's in `[tool.uv.pip]`, it will take preference for `uv pip` operations.


---

_Label `configuration` added by @charliermarsh on 2025-12-13 02:46_

---

_Marked ready for review by @charliermarsh on 2025-12-13 02:47_

---

_Merged by @charliermarsh on 2025-12-16 01:18_

---

_Closed by @charliermarsh on 2025-12-16 01:18_

---

_Branch deleted on 2025-12-16 01:18_

---

_Comment by @84sdyer on 2025-12-19 14:40_

gh pr checkout 17116


---
