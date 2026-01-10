```yaml
number: 8012
title: "Avoid deleting a project environment directory if we cannot tell if a `pyvenv.cfg` file exists"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/venv-replace
created_at: 2024-10-08T17:22:52Z
updated_at: 2024-10-08T22:15:49Z
url: https://github.com/astral-sh/uv/pull/8012
synced_at: 2026-01-10T12:54:01Z
```

# Avoid deleting a project environment directory if we cannot tell if a `pyvenv.cfg` file exists

---

_Pull request opened by @zanieb on 2024-10-08 17:22_

I was exploring a fix to an [apparent bug](https://github.com/astral-sh/uv/actions/runs/11240101202/job/31248937280?pr=8010) but this was actually just a CI change https://github.com/astral-sh/uv/pull/8013

Regardless, I think this code is safer?

---

_Label `bug` added by @zanieb on 2024-10-08 17:22_

---

_Marked ready for review by @zanieb on 2024-10-08 17:38_

---

_@charliermarsh approved on 2024-10-08 22:09_

---

_Merged by @zanieb on 2024-10-08 22:15_

---

_Closed by @zanieb on 2024-10-08 22:15_

---

_Branch deleted on 2024-10-08 22:15_

---
