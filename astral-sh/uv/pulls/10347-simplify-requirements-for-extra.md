```yaml
number: 10347
title: "Simplify `requirements_for_extra`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/simplify-requirements-for-extras
created_at: 2025-01-07T09:34:22Z
updated_at: 2025-01-07T13:50:24Z
url: https://github.com/astral-sh/uv/pull/10347
synced_at: 2026-01-12T16:09:15Z
```

# Simplify `requirements_for_extra`

---

_@konstin_

Ref https://github.com/astral-sh/uv/issues/10344

Not a performance optimization, but the function had become too large. No logic changes, just code moving around. Looks slightly better when ignoring whitespace changes.

It's still too complex but i haven't found an apt simplification.


---

_Label `internal` added by @konstin on 2025-01-07 09:34_

---

_@charliermarsh approved on 2025-01-07 13:50_

---

_Merged by @charliermarsh on 2025-01-07 13:50_

---

_Closed by @charliermarsh on 2025-01-07 13:50_

---

_Branch deleted on 2025-01-07 13:50_

---
