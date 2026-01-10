---
number: 1196
title: Bootstrap scripts fails on macOS
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-01-30T22:54:00Z
updated_at: 2024-01-31T15:22:15Z
url: https://github.com/astral-sh/uv/issues/1196
synced_at: 2026-01-10T01:23:05Z
---

# Bootstrap scripts fails on macOS

---

_Issue opened by @charliermarsh on 2024-01-30 22:54_

```shell
❯ scripts/bootstrap/install.sh
scripts/bootstrap/install.sh: line 83: readarray: command not found
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-30 23:04_

---

_Referenced in [astral-sh/uv#1198](../../astral-sh/uv/pulls/1198.md) on 2024-01-31 00:21_

---

_Label `bug` added by @charliermarsh on 2024-01-31 00:32_

---

_Comment by @zanieb on 2024-01-31 14:45_

What bash version do you have?

---

_Comment by @zanieb on 2024-01-31 14:45_

```
❯ bash --version
GNU bash, version 5.2.21(1)-release (aarch64-apple-darwin23.0.0)

---

_Comment by @charliermarsh on 2024-01-31 15:22_

```
❯ bash --version
GNU bash, version 3.2.57(1)-release (arm64-apple-darwin23)
```

My understanding is that Apple ships an older Bash by default due to licensing reasons?

---

_Closed by @charliermarsh on 2024-01-31 15:22_

---
