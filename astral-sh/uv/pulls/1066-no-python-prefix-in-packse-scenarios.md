```yaml
number: 1066
title: No python prefix in packse scenarios
type: pull_request
state: merged
author: konstin
labels:
  - windows
assignees: []
merged: true
base: main
head: konsti/package-venv-with-python-prefix
created_at: 2024-01-23T16:44:38Z
updated_at: 2024-01-24T11:22:49Z
url: https://github.com/astral-sh/uv/pull/1066
synced_at: 2026-01-12T16:04:23Z
```

# No python prefix in packse scenarios

---

_@konstin_

In windows, `python3.9` and `python3.11` are not in `PATH`. Instead, we should pass only the python version to `puffin venv -p` in packse scenarios (#1039).

---

_Review requested from @zanieb by @konstin on 2024-01-23 16:44_

---

_Label `windows` added by @konstin on 2024-01-23 16:44_

---

_@zanieb approved on 2024-01-23 16:58_

---

_Merged by @konstin on 2024-01-24 11:22_

---

_Closed by @konstin on 2024-01-24 11:22_

---

_Branch deleted on 2024-01-24 11:22_

---
