```yaml
number: 1334
title: "Add `pip install --constraint` test coverage"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/install-constraint
created_at: 2024-02-15T20:43:37Z
updated_at: 2024-02-16T17:39:40Z
url: https://github.com/astral-sh/uv/pull/1334
synced_at: 2026-01-10T15:33:24Z
```

# Add `pip install --constraint` test coverage

---

_Pull request opened by @zanieb on 2024-02-15 20:43_

Exploring behavior reported in https://github.com/astral-sh/uv/issues/1332

---

_@charliermarsh reviewed on 2024-02-15 21:20_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:1072 on 2024-02-15 21:20_

This is the bug, right?

---

_@zanieb reviewed on 2024-02-15 23:25_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1072 on 2024-02-15 23:25_

It sure looks wrong to me.

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1072 on 2024-02-16 17:20_

Looking into this.. really weird silent failure in parsing?

---

_@zanieb reviewed on 2024-02-16 17:20_

---

_Label `testing` added by @zanieb on 2024-02-16 17:37_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1072 on 2024-02-16 17:37_

Bad test — looks fine https://github.com/astral-sh/uv/pull/1334/commits/81c466c16c556dce872223e8ed8927d85759b7b0

---

_@zanieb reviewed on 2024-02-16 17:37_

---

_Merged by @zanieb on 2024-02-16 17:39_

---

_Closed by @zanieb on 2024-02-16 17:39_

---

_Branch deleted on 2024-02-16 17:39_

---
