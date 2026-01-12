```yaml
number: 7688
title: "Rename `uv-build` to `uv-build-frontend`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/uv-build-frontend
created_at: 2024-09-25T16:25:47Z
updated_at: 2024-09-25T18:17:56Z
url: https://github.com/astral-sh/uv/pull/7688
synced_at: 2026-01-12T16:07:57Z
```

# Rename `uv-build` to `uv-build-frontend`

---

_@konstin_

uv will soon support both a build frontend (`uv build`) and a build backend (`build-system = "uv"`). To avoid the name clash, I'm renaming the `uv-build` crate to `uv-build-frontend`. In a follow-up PR, I will add a `uv-build-backend` crate with the build backend implementation.


---

_Label `internal` added by @konstin on 2024-09-25 16:25_

---

_Review requested from @charliermarsh by @konstin on 2024-09-25 16:25_

---

_@zanieb approved on 2024-09-25 16:26_

---

_Comment by @charliermarsh on 2024-09-25 18:17_

Gonna merge this to avoid conflicts.

---

_Merged by @charliermarsh on 2024-09-25 18:17_

---

_Closed by @charliermarsh on 2024-09-25 18:17_

---

_Branch deleted on 2024-09-25 18:17_

---
