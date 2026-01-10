```yaml
number: 3945
title: "Recreate project environment if `--python` or `requires-python` doesn't match"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/take-interpreter-into-account
created_at: 2024-05-31T17:34:49Z
updated_at: 2024-06-10T14:36:15Z
url: https://github.com/astral-sh/uv/pull/3945
synced_at: 2026-01-10T13:54:02Z
```

# Recreate project environment if `--python` or `requires-python` doesn't match

---

_Pull request opened by @konstin on 2024-05-31 17:34_

Fixes #4131
Fixes #3895

---

_Label `preview` added by @konstin on 2024-05-31 17:34_

---

_Marked ready for review by @konstin on 2024-06-10 09:02_

---

_Comment by @konstin on 2024-06-10 09:03_

I have to check windows is-satisfied detection but otherwise this is ready for review

---

_@charliermarsh reviewed on 2024-06-10 13:07_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/discovery.rs`:952 on 2024-06-10 13:07_

Nit: can we reverse the order of these arguments to match the `ToolchainRequest::Directory` case?

---

_@charliermarsh reviewed on 2024-06-10 13:08_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:145 on 2024-06-10 13:08_

Can we `if`-`else` this warning? `you requested with (default) is incompatible...` is confusing and it's trivial for us to use a better message here.

---

_@charliermarsh reviewed on 2024-06-10 13:09_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:143 on 2024-06-10 13:09_

Nit: `: {}` instead of `of {}`.

---

_@charliermarsh reviewed on 2024-06-10 13:09_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/discovery.rs`:22 on 2024-06-10 13:09_

Import sections here are all over the place lol.

---

_@charliermarsh approved on 2024-06-10 13:09_

---

_@zanieb reviewed on 2024-06-10 13:41_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:22 on 2024-06-10 13:41_

Sorry that's probably my fault I use the editors "add import" feature a lot and things can get out of hand.

---

_@zanieb reviewed on 2024-06-10 13:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:52 on 2024-06-10 13:42_

I guess I should update this API to take the `ToolchainRequest` instead of a string.

---

_@zanieb reviewed on 2024-06-10 13:44_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:14 on 2024-06-10 13:44_

Thanks for writing a test!

---

_@zanieb approved on 2024-06-10 13:44_

---

_Comment by @charliermarsh on 2024-06-10 14:09_

I am debugging the Windows failure, since I want to merge this for today's release.

---

_Renamed from "Recreate venv in bluejay if `--python` or `requires_python` doesn't match" to "Recreate project environment if `--python` or `requires-python` doesn't match" by @charliermarsh on 2024-06-10 14:25_

---

_Comment by @charliermarsh on 2024-06-10 14:28_

Hopefully fixed? Fixed it locally, at least.

---

_Merged by @charliermarsh on 2024-06-10 14:36_

---

_Closed by @charliermarsh on 2024-06-10 14:36_

---

_Branch deleted on 2024-06-10 14:36_

---
