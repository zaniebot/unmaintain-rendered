```yaml
number: 15671
title: "Avoid erroring when `match-runtime` target is optional"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/match
created_at: 2025-09-04T02:34:36Z
updated_at: 2025-09-04T12:34:55Z
url: https://github.com/astral-sh/uv/pull/15671
synced_at: 2026-01-12T16:11:53Z
```

# Avoid erroring when `match-runtime` target is optional

---

_@charliermarsh_


## Summary

If the package that has the `match-runtime` dependency itself isn't being installed, we should avoid erroring if the package it _depends on_ isn't in the resolution.

Closes https://github.com/astral-sh/uv/issues/15661.


---

_Label `bug` added by @charliermarsh on 2025-09-04 02:34_

---

_Review requested from @zanieb by @charliermarsh on 2025-09-04 02:34_

---

_Marked ready for review by @charliermarsh on 2025-09-04 02:34_

---

_@zanieb approved on 2025-09-04 02:38_

---

_Merged by @charliermarsh on 2025-09-04 12:34_

---

_Closed by @charliermarsh on 2025-09-04 12:34_

---

_Branch deleted on 2025-09-04 12:34_

---
