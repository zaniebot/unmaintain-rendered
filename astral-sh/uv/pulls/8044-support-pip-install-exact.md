```yaml
number: 8044
title: "Support `pip install --exact`"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: pip-install-exact
created_at: 2024-10-09T11:25:43Z
updated_at: 2024-10-09T13:31:34Z
url: https://github.com/astral-sh/uv/pull/8044
synced_at: 2026-01-10T12:54:01Z
```

# Support `pip install --exact`

---

_Pull request opened by @blueraft on 2024-10-09 11:25_

## Summary

Resolves #8041 

## Test Plan

`cargo test`


---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-09 12:35_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-10-09 12:35_

---

_Label `enhancement` added by @charliermarsh on 2024-10-09 12:35_

---

_Label `cli` added by @charliermarsh on 2024-10-09 12:35_

---

_@charliermarsh approved on 2024-10-09 12:52_

Thank you, so fast!

---

_@blueraft reviewed on 2024-10-09 13:07_

---

_Review comment by @blueraft on `crates/uv/src/settings.rs`:1404 on 2024-10-09 13:07_

I think this should default to false. I've made the change, and the tests pass for me locally.

---

_Merged by @charliermarsh on 2024-10-09 13:31_

---

_Closed by @charliermarsh on 2024-10-09 13:31_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:1404 on 2024-10-09 13:31_

Thanks, my bad.

---

_@charliermarsh reviewed on 2024-10-09 13:31_

---
