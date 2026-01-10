```yaml
number: 915
title: Normalize extras when evaluating PEP 508 markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/extra
created_at: 2024-01-14T16:08:16Z
updated_at: 2024-01-14T17:16:55Z
url: https://github.com/astral-sh/uv/pull/915
synced_at: 2026-01-10T15:39:02Z
```

# Normalize extras when evaluating PEP 508 markers

---

_Pull request opened by @charliermarsh on 2024-01-14 16:08_

## Summary

We always normalize extra names in our requirements (e.g., `cuda12_pip` to `cuda12-pip`), but we weren't normalizing within PEP 508 markers, which meant we ended up comparing `cuda12-pip` (normalized) against `cuda12_pip` (unnormalized).

Closes https://github.com/astral-sh/puffin/issues/911.


---

_Label `bug` added by @charliermarsh on 2024-01-14 16:08_

---

_Review requested from @konstin by @charliermarsh on 2024-01-14 16:08_

---

_Comment by @charliermarsh on 2024-01-14 16:08_

This needs a scenario...

---

_Comment by @zanieb on 2024-01-14 16:11_

I'll add support for extras, I don't think that exists yet.

---

_@konstin approved on 2024-01-14 16:48_

---

_Merged by @charliermarsh on 2024-01-14 17:16_

---

_Closed by @charliermarsh on 2024-01-14 17:16_

---

_Branch deleted on 2024-01-14 17:16_

---
