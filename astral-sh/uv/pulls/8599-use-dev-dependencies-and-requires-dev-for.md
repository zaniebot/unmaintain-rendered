```yaml
number: 8599
title: "Use `dev-dependencies` and `requires-dev` for lockfile compatibility"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/back
created_at: 2024-10-26T17:41:41Z
updated_at: 2024-10-28T22:08:34Z
url: https://github.com/astral-sh/uv/pull/8599
synced_at: 2026-01-10T12:54:13Z
```

# Use `dev-dependencies` and `requires-dev` for lockfile compatibility

---

_Pull request opened by @charliermarsh on 2024-10-26 17:41_

## Summary

Unfortunately, it looks like we lost https://github.com/astral-sh/uv/pull/8501 somewhere in a bad rebase. This PR re-adds the change, with compatibility for those lockfiles created in v0.4.27. I'm not certain we should actually merge this. It might be less painful and confusing to just bite the bullet on the change.


---

_Label `compatibility` added by @charliermarsh on 2024-10-26 17:41_

---

_Review requested from @zanieb by @charliermarsh on 2024-10-26 17:41_

---

_@zanieb approved on 2024-10-28 22:03_

Without this, people on older versions of uv could silently lose their development dependencies (which seems problematic enough that we should make this change despite the churn).

---

_Merged by @zanieb on 2024-10-28 22:08_

---

_Closed by @zanieb on 2024-10-28 22:08_

---

_Branch deleted on 2024-10-28 22:08_

---
