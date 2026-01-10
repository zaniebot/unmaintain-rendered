```yaml
number: 4283
title: Avoid treating direct path archives as always dynamic
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/repeated
created_at: 2024-06-12T17:17:31Z
updated_at: 2024-06-12T17:28:31Z
url: https://github.com/astral-sh/uv/pull/4283
synced_at: 2026-01-10T13:54:02Z
```

# Avoid treating direct path archives as always dynamic

---

_Pull request opened by @charliermarsh on 2024-06-12 17:17_

## Summary

Right now, we're _always_ reinstalling local wheel archives, even if the timestamp didn't change.

I want to fix the TODO properly but I will do so in a separate PR.


---

_Label `bug` added by @charliermarsh on 2024-06-12 17:17_

---

_@zanieb reviewed on 2024-06-12 17:18_

---

_Review comment by @zanieb on `crates/uv-installer/src/satisfies.rs`:210 on 2024-06-12 17:18_

Better message here?

---

_@zanieb approved on 2024-06-12 17:18_

---

_@charliermarsh reviewed on 2024-06-12 17:21_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/satisfies.rs`:210 on 2024-06-12 17:21_

Oh sorry I thought this was already present.

---

_Merged by @charliermarsh on 2024-06-12 17:28_

---

_Closed by @charliermarsh on 2024-06-12 17:28_

---

_Branch deleted on 2024-06-12 17:28_

---
