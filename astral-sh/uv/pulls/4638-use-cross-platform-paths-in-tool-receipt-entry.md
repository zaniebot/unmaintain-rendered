```yaml
number: 4638
title: Use cross-platform paths in tool receipt entry point paths
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: zb/tool-receipt-format
head: zb/tool-receipt-path-platform
created_at: 2024-06-28T21:29:48Z
updated_at: 2024-06-29T03:34:00Z
url: https://github.com/astral-sh/uv/pull/4638
synced_at: 2026-01-10T13:48:28Z
```

# Use cross-platform paths in tool receipt entry point paths

---

_Pull request opened by @zanieb on 2024-06-28 21:29_

As https://github.com/astral-sh/uv/pull/4324 did for lock files, updates the tool receipts to use portable paths. This resolves CI failures on Windows in #4634 where the toml string literal type changes depending on the platform.

---

_Marked ready for review by @zanieb on 2024-06-28 21:33_

---

_Label `preview` added by @zanieb on 2024-06-28 21:33_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-28 21:59_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-28 21:59_

---

_Comment by @zanieb on 2024-06-28 22:26_

I'll merge this all downstack into #4634 so we can merge something that passes CI but feel free to review separately.

---

_@charliermarsh approved on 2024-06-28 23:45_

---

_Merged by @zanieb on 2024-06-29 03:33_

---

_Closed by @zanieb on 2024-06-29 03:33_

---

_Branch deleted on 2024-06-29 03:34_

---
