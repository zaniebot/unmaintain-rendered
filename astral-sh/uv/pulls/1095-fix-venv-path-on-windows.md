```yaml
number: 1095
title: Fix venv PATH on windows
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: konsti/fix-venv-path-windows
created_at: 2024-01-25T15:38:59Z
updated_at: 2024-01-25T15:40:54Z
url: https://github.com/astral-sh/uv/pull/1095
synced_at: 2026-01-12T16:04:25Z
```

# Fix venv PATH on windows

---

_@konstin_

Windows uses `;` instead of `:` to separate `PATH` entries. This pull request switches from manually using `:` to the `std::env` functions. This fixes

```
puffin pip install -e scripts/editable-installs/maturin_editable
```

on windows.

---

_Label `bug` added by @konstin on 2024-01-25 15:38_

---

_Label `windows` added by @konstin on 2024-01-25 15:38_

---

_Merged by @konstin on 2024-01-25 15:40_

---

_Closed by @konstin on 2024-01-25 15:40_

---

_Branch deleted on 2024-01-25 15:40_

---
