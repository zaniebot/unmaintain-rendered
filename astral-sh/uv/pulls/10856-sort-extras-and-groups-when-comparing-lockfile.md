```yaml
number: 10856
title: Sort extras and groups when comparing lockfile requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2025-01-22T13:58:42Z
updated_at: 2025-01-22T17:06:07Z
url: https://github.com/astral-sh/uv/pull/10856
synced_at: 2026-01-12T16:09:31Z
```

# Sort extras and groups when comparing lockfile requirements

---

_@charliermarsh_

## Summary

The linked issue actually isn't a bug on main anymore, but it does require us to take the "slow" path, since setuptools seems to reorder the extras. This PR adds another normalization step which lets us take the fast path: https://github.com/astral-sh/uv/issues/10855.


---

_Label `performance` added by @charliermarsh on 2025-01-22 13:58_

---

_@zanieb reviewed on 2025-01-22 16:20_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:1321 on 2025-01-22 16:20_

Intended?

---

_@zanieb approved on 2025-01-22 16:20_

---

_@charliermarsh reviewed on 2025-01-22 16:51_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:1321 on 2025-01-22 16:51_

Yeah

---

_Merged by @charliermarsh on 2025-01-22 17:06_

---

_Closed by @charliermarsh on 2025-01-22 17:06_

---

_Branch deleted on 2025-01-22 17:06_

---
