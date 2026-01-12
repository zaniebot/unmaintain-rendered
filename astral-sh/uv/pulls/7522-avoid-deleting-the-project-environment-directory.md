```yaml
number: 7522
title: Avoid deleting the project environment directory if it is not a virtual environment
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-project-env-existing
created_at: 2024-09-18T23:32:28Z
updated_at: 2024-09-19T11:21:06Z
url: https://github.com/astral-sh/uv/pull/7522
synced_at: 2026-01-12T16:07:52Z
```

# Avoid deleting the project environment directory if it is not a virtual environment

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/7519

---

_Label `bug` added by @zanieb on 2024-09-18 23:32_

---

_Review requested from @charliermarsh by @zanieb on 2024-09-18 23:50_

---

_@zanieb reviewed on 2024-09-18 23:51_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:497 on 2024-09-18 23:51_

We do something far more complex for `uv venv` which has a similar error case

https://github.com/astral-sh/uv/blob/f895c40a4ec00a0e27a8f81648ccb8e9ccc4780d/crates/uv-virtualenv/src/virtualenv.rs#L88-L122

Should we split that out and replicate it? Is it necessary here?

---

_Comment by @zanieb on 2024-09-19 01:09_

Going to have to fight with Windows CI

---

_@charliermarsh reviewed on 2024-09-19 01:26_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:496 on 2024-09-19 01:26_

Should this part be catching a more targeted error?

---

_@charliermarsh reviewed on 2024-09-19 01:26_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:497 on 2024-09-19 01:26_

I do like the more explicit error handling like we have there.

---

_@charliermarsh approved on 2024-09-19 01:26_

---

_@zanieb reviewed on 2024-09-19 01:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:496 on 2024-09-19 01:37_

Naw we just care that it is not a virtual environment. I think this is addressed in #7527 anyway since I add a real error variant for invalid environments.

---

_@zanieb reviewed on 2024-09-19 01:39_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:497 on 2024-09-19 01:39_

I think this also will be partially resolved by #7527, I ported at least some of these checks. I can revisit shared logic later too.

---

_Merged by @zanieb on 2024-09-19 11:21_

---

_Closed by @zanieb on 2024-09-19 11:21_

---

_Branch deleted on 2024-09-19 11:21_

---
