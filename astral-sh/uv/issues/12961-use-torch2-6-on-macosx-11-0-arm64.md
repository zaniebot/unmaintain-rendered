---
number: 12961
title: Use torch2.6 on macosx_11_0_arm64
type: issue
state: closed
author: xnuohz
labels:
  - question
assignees: []
created_at: 2025-04-18T05:55:36Z
updated_at: 2025-04-21T01:38:31Z
url: https://github.com/astral-sh/uv/issues/12961
synced_at: 2026-01-10T01:25:27Z
---

# Use torch2.6 on macosx_11_0_arm64

---

_Issue opened by @xnuohz on 2025-04-18 05:55_

### Question

should uv provide python version like `cpython-3.12.10-macos-arm64-none`?

### Platform

Darwin 24.2.0 arm64

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

---

_Label `question` added by @xnuohz on 2025-04-18 05:55_

---

_Comment by @charliermarsh on 2025-04-21 01:38_

We do provide that:

```
❯ uv python install 3.12.10
Installed Python 3.12.10 in 745ms
 + cpython-3.12.10-macos-aarch64-none
```

---

_Closed by @charliermarsh on 2025-04-21 01:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-21 01:38_

---
