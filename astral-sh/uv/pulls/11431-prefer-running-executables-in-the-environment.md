```yaml
number: 11431
title: "Prefer running executables in the environment with `<name>` over `<name>/__main__.py`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/run-module
created_at: 2025-02-11T22:10:07Z
updated_at: 2025-02-12T18:08:59Z
url: https://github.com/astral-sh/uv/pull/11431
synced_at: 2026-01-10T11:10:38Z
```

# Prefer running executables in the environment with `<name>` over `<name>/__main__.py`

---

_Pull request opened by @zanieb on 2025-02-11 22:10_

Closes https://github.com/astral-sh/uv/issues/11423
Closes https://github.com/astral-sh/uv/issues/9167
Closes https://github.com/astral-sh/uv/pull/9722

https://github.com/astral-sh/uv/pull/11431/commits/c23fc4024e1f4d3315ec385a2a5a3c6be1e2dae5 demonstrates the behavior.

---

_Label `bug` added by @zanieb on 2025-02-11 22:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1215 on 2025-02-11 22:17_

https://github.com/astral-sh/uv/pull/9722 implements an alternative approach using more types. I think that's nice in theory, but once I reviewed it and attempted to implemented it, it didn't feel worth the indirection cost. This issue is relatively specific — let's deal with it here for now.

---

_@zanieb reviewed on 2025-02-11 22:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1487 on 2025-02-12 00:13_

What happens if an entrypoint exists, but there is no `__main__.py` file? Do we still run the entrypoint as expected?

---

_@charliermarsh reviewed on 2025-02-12 00:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1487 on 2025-02-12 04:49_

If I follow what you're saying here, yes this is covered in the first snapshot of the test case. The only behavior that's changing here is when a `__main__.py` file is present.

---

_@zanieb reviewed on 2025-02-12 04:49_

---

_Review requested from @charliermarsh by @zanieb on 2025-02-12 15:59_

---

_@charliermarsh approved on 2025-02-12 18:03_

---

_Merged by @zanieb on 2025-02-12 18:08_

---

_Closed by @zanieb on 2025-02-12 18:08_

---

_Branch deleted on 2025-02-12 18:08_

---
