```yaml
number: 4360
title: "Respect `.python-version` in `uv venv --preview`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-file-venv
created_at: 2024-06-17T16:40:47Z
updated_at: 2024-06-18T14:21:36Z
url: https://github.com/astral-sh/uv/pull/4360
synced_at: 2026-01-10T13:54:02Z
```

# Respect `.python-version` in `uv venv --preview`

---

_Pull request opened by @zanieb on 2024-06-17 16:40_

Adds support for reading Python version files (introduced in #4335) to `uv venv`. If present, we'll use the file version as the default.

---

_Label `preview` added by @zanieb on 2024-06-17 16:40_

---

_@zanieb reviewed on 2024-06-17 16:43_

---

_Review comment by @zanieb on `.python-versions`:1 on 2024-06-17 16:43_

Reversed the ordering here because we want this to be the "first" version we use.

---

_@konstin approved on 2024-06-17 16:44_

---

_@ibraheemdev approved on 2024-06-17 18:18_

---

_Merged by @zanieb on 2024-06-18 14:21_

---

_Closed by @zanieb on 2024-06-18 14:21_

---

_Branch deleted on 2024-06-18 14:21_

---
