```yaml
number: 10333
title: Sync latest Python releases
type: pull_request
state: merged
author: github-actions
labels:
  - enhancement
assignees: []
merged: true
base: main
head: sync-python-releases
created_at: 2025-01-06T19:03:06Z
updated_at: 2025-01-06T19:19:37Z
url: https://github.com/astral-sh/uv/pull/10333
synced_at: 2026-01-12T16:09:14Z
```

# Sync latest Python releases

---

_@github-actions_

Automated update for Python releases.

---

_Closed by @zanieb on 2025-01-06 19:03_

---

_Reopened by @zanieb on 2025-01-06 19:03_

---

_Comment by @zanieb on 2025-01-06 19:14_

```
❯ ./target/debug/uv python install 3.14
Installed Python 3.14.0a3 in 1.67s
 + cpython-3.14.0a3-macos-aarch64-none
❯ ./target/debug/uv python install '>=3.13'
cpython-3.13.1-macos-aarch64-none ------------------------------ 7.45 MiB/14.78 MiB                                          
^C
❯ ./target/debug/uv python install 3.14 --preview --default
Installed Python 3.14.0a3 in 32ms
 + cpython-3.14.0a3-macos-aarch64-none (python, python3, python3.14)
❯ python --version
Python 3.14.0a3
```

---

_@zanieb approved on 2025-01-06 19:18_

---

_Label `enhancement` added by @zanieb on 2025-01-06 19:19_

---

_Merged by @zanieb on 2025-01-06 19:19_

---

_Closed by @zanieb on 2025-01-06 19:19_

---

_Branch deleted on 2025-01-06 19:19_

---
