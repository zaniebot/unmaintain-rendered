```yaml
number: 4520
title: Copy tool entrypoints on Windows instead of using a symbolic link
type: pull_request
state: merged
author: zanieb
labels:
  - windows
  - preview
assignees: []
merged: true
base: zb/tool-install-test
head: zb/tool-install-windows
created_at: 2024-06-25T15:57:33Z
updated_at: 2024-06-26T15:23:20Z
url: https://github.com/astral-sh/uv/pull/4520
synced_at: 2026-01-12T16:06:17Z
```

# Copy tool entrypoints on Windows instead of using a symbolic link

---

_@zanieb_

This matches [pipx's behavior](https://github.com/pypa/pipx/blob/4d8c05884317f7b646a66ffd4e933f4d1df471b7/src/pipx/commands/common.py#L69-L70). Includes test changes to get past some blockers on Windows.

Resolves failures on #4509


---

_Renamed from "Copy tools on Windows instead of using a symbolic link" to "Copy tool entrypoints on Windows instead of using a symbolic link" by @zanieb on 2024-06-25 15:58_

---

_Label `preview` added by @zanieb on 2024-06-25 16:13_

---

_Label `preview` added by @zanieb on 2024-06-25 16:13_

---

_Label `windows` added by @zanieb on 2024-06-25 16:46_

---

_@charliermarsh approved on 2024-06-26 00:47_

---

_Merged by @zanieb on 2024-06-26 15:23_

---

_Closed by @zanieb on 2024-06-26 15:23_

---

_Branch deleted on 2024-06-26 15:23_

---
