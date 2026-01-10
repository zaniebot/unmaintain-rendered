```yaml
number: 5453
title: Allow distributions to be absent in deserialization
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2024-07-25T17:31:08Z
updated_at: 2024-07-25T18:01:42Z
url: https://github.com/astral-sh/uv/pull/5453
synced_at: 2026-01-10T13:37:23Z
```

# Allow distributions to be absent in deserialization

---

_Pull request opened by @charliermarsh on 2024-07-25 17:31_

## Summary

Closes https://github.com/astral-sh/uv/issues/5434.


---

_Label `bug` added by @charliermarsh on 2024-07-25 17:31_

---

_Label `preview` added by @charliermarsh on 2024-07-25 17:31_

---

_Marked ready for review by @charliermarsh on 2024-07-25 17:31_

---

_Review comment by @zanieb on `crates/uv/tests/sync.rs`:186 on 2024-07-25 17:34_

Maybe we should throw a requires-python in there to reduce the noise?

---

_@zanieb reviewed on 2024-07-25 17:34_

---

_@zanieb approved on 2024-07-25 17:34_

---

_@charliermarsh reviewed on 2024-07-25 17:48_

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:186 on 2024-07-25 17:48_

Not sure if we "can" since it's a virtual workspace (not a project). I'll try.

---

_@charliermarsh reviewed on 2024-07-25 17:48_

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:186 on 2024-07-25 17:48_

Typically it would have at least one member.

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:186 on 2024-07-25 17:51_

Yeah as soon as we add a `project` field it becomes non-virtual and the package itself gets included in the lockfile (making it non-empty).

---

_@charliermarsh reviewed on 2024-07-25 17:51_

---

_Merged by @charliermarsh on 2024-07-25 18:01_

---

_Closed by @charliermarsh on 2024-07-25 18:01_

---

_Branch deleted on 2024-07-25 18:01_

---
