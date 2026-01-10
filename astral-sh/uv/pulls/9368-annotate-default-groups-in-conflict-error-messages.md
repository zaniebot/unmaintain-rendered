```yaml
number: 9368
title: Annotate default groups in conflict error messages
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/default-message
created_at: 2024-11-22T20:19:16Z
updated_at: 2024-11-22T21:13:17Z
url: https://github.com/astral-sh/uv/pull/9368
synced_at: 2026-01-10T12:00:00Z
```

# Annotate default groups in conflict error messages

---

_Pull request opened by @charliermarsh on 2024-11-22 20:19_

## Summary

We now tell the user if a group was enabled by default.

Closes https://github.com/astral-sh/uv/issues/9366.


---

_Label `error messages` added by @charliermarsh on 2024-11-22 20:19_

---

_@zanieb reviewed on 2024-11-22 20:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:3789 on 2024-11-22 20:26_

As in the other pull request, can we name these differently please?

---

_@zanieb approved on 2024-11-22 20:26_

Love this :)

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:232 on 2024-11-22 20:26_

Another option is just "default group `{group}`"

---

_@zanieb reviewed on 2024-11-22 20:26_

---

_@charliermarsh reviewed on 2024-11-22 20:28_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:3789 on 2024-11-22 20:28_

This series of tests all use this style, I guess I'd rather do it in a single PR and change all of them.

---

_@zanieb reviewed on 2024-11-22 21:03_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:3789 on 2024-11-22 21:03_

Yeah no problem.

---

_Merged by @charliermarsh on 2024-11-22 21:13_

---

_Closed by @charliermarsh on 2024-11-22 21:13_

---

_Branch deleted on 2024-11-22 21:13_

---
