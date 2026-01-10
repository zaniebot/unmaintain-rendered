```yaml
number: 2196
title: "Error when direct URL requirements don't match `Requires-Python`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/req-ii
created_at: 2024-03-05T04:41:01Z
updated_at: 2024-03-14T02:37:02Z
url: https://github.com/astral-sh/uv/pull/2196
synced_at: 2026-01-10T14:49:08Z
```

# Error when direct URL requirements don't match `Requires-Python`

---

_Pull request opened by @charliermarsh on 2024-03-05 04:41_

## Summary

Closes https://github.com/astral-sh/uv/issues/2195.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2024-03-05 04:41_

---

_Label `error messages` added by @charliermarsh on 2024-03-05 04:41_

---

_@charliermarsh reviewed on 2024-03-05 04:41_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:600 on 2024-03-05 04:41_

We have to get rid of this "fast path", because we now need the `Requires-Python` here.

Alternatively, we could move the `Requires-Python` check into `get_dependencies` for URL dependencies.

---

_Marked ready for review by @charliermarsh on 2024-03-05 04:46_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-07 04:40_

---

_Review request for @zanieb removed by @charliermarsh on 2024-03-11 21:59_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-11 21:59_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:2999 on 2024-03-13 14:35_

Why does this one still pass?

---

_@zanieb reviewed on 2024-03-13 14:35_

---

_@charliermarsh reviewed on 2024-03-13 15:02_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:2999 on 2024-03-13 15:02_

I honestly don't remember, let me try rebasing.

---

_@charliermarsh reviewed on 2024-03-13 15:02_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:2999 on 2024-03-13 15:02_

Oh, I think we don't enforce this in `pip-sync`.

---

_@charliermarsh reviewed on 2024-03-13 15:03_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:2999 on 2024-03-13 15:03_

I can't remember why. I think the separation somewhere in the pipeline made it hard.

---

_@zanieb reviewed on 2024-03-13 20:03_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:2999 on 2024-03-13 20:03_

Should we track this somewhere?

---

_@zanieb approved on 2024-03-13 20:03_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:2999 on 2024-03-14 02:28_

#2443

---

_@charliermarsh reviewed on 2024-03-14 02:28_

---

_Merged by @charliermarsh on 2024-03-14 02:37_

---

_Closed by @charliermarsh on 2024-03-14 02:37_

---

_Branch deleted on 2024-03-14 02:37_

---
