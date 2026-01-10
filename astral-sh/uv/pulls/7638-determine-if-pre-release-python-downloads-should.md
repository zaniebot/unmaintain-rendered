```yaml
number: 7638
title: Determine if pre-release Python downloads should be allowed using the version specifiers
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-pre-install
created_at: 2024-09-23T13:35:13Z
updated_at: 2024-09-23T19:09:02Z
url: https://github.com/astral-sh/uv/pull/7638
synced_at: 2026-01-10T12:53:52Z
```

# Determine if pre-release Python downloads should be allowed using the version specifiers

---

_Pull request opened by @zanieb on 2024-09-23 13:35_

Closes #7637

```
❯ uv python install '>=3.11'
Installed Python 3.12.6 in 1.70s
 + cpython-3.12.6-macos-aarch64-none

❯ uv python install 3.13
Installed Python 3.13.0rc2 in 1.89s
 + cpython-3.13.0rc2-macos-aarch64-none

❯ uv python uninstall --all
Searching for Python installations
Uninstalled 2 versions in 94ms
 - cpython-3.12.6-macos-aarch64-none
 - cpython-3.13.0rc2-macos-aarch64-none

❯ uv python install '>=3.11a1'
Installed Python 3.13.0rc2 in 1.89s
 + cpython-3.13.0rc2-macos-aarch64-none
```

---

_Label `bug` added by @zanieb on 2024-09-23 13:35_

---

_Merged by @zanieb on 2024-09-23 19:08_

---

_Closed by @zanieb on 2024-09-23 19:09_

---

_Branch deleted on 2024-09-23 19:09_

---
