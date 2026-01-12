```yaml
number: 10348
title: "Optimize `requirements_for_extra`"
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/optimize-requirements-for-extras
created_at: 2025-01-07T09:37:56Z
updated_at: 2025-01-07T14:12:13Z
url: https://github.com/astral-sh/uv/pull/10348
synced_at: 2026-01-12T16:09:15Z
```

# Optimize `requirements_for_extra`

---

_@konstin_

Ref https://github.com/astral-sh/uv/issues/10344

Avoid recomputing the python marker inside the loop.

Check the faster conditions in `is_requirement_applicable` first.

---

_Label `performance` added by @konstin on 2025-01-07 09:38_

---

_@charliermarsh approved on 2025-01-07 13:55_

---

_Merged by @konstin on 2025-01-07 14:12_

---

_Closed by @konstin on 2025-01-07 14:12_

---

_Branch deleted on 2025-01-07 14:12_

---
