```yaml
number: 4074
title: "Add regression test for `VIRTUAL_ENV` and `uv venv` interaction"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/fix-venv-reg
created_at: 2024-06-05T20:55:07Z
updated_at: 2024-06-06T00:28:46Z
url: https://github.com/astral-sh/uv/pull/4074
synced_at: 2026-01-12T16:06:01Z
```

# Add regression test for `VIRTUAL_ENV` and `uv venv` interaction

---

_@zanieb_

For https://github.com/astral-sh/uv/pull/4073

Demonstrated to fail without those changes.

---

_Label `testing` added by @zanieb on 2024-06-05 20:55_

---

_@charliermarsh approved on 2024-06-05 20:56_

---

_@charliermarsh reviewed on 2024-06-05 20:56_

---

_Review comment by @charliermarsh on `crates/uv/tests/venv.rs`:167 on 2024-06-05 20:56_

Can you add a line stating why we don't care?

---

_@zanieb reviewed on 2024-06-05 21:01_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:167 on 2024-06-05 21:01_

Definitely

---

_Merged by @zanieb on 2024-06-06 00:28_

---

_Closed by @zanieb on 2024-06-06 00:28_

---

_Branch deleted on 2024-06-06 00:28_

---
