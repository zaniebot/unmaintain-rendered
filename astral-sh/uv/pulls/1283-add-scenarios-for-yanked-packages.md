```yaml
number: 1283
title: Add scenarios for yanked packages
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/yanked
created_at: 2024-02-12T17:31:23Z
updated_at: 2024-02-12T19:17:08Z
url: https://github.com/astral-sh/uv/pull/1283
synced_at: 2026-01-12T16:04:33Z
```

# Add scenarios for yanked packages

---

_@zanieb_

_No description provided._

---

_@zanieb reviewed on 2024-02-12 17:31_

---

_Review comment by @zanieb on `scripts/scenarios/update.py`:168 on 2024-02-12 17:31_

This scenario has the wrong outcome and I need to debug why but it is not part of this pull request

---

_@zanieb reviewed on 2024-02-12 17:33_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_install_scenarios.rs`:785 on 2024-02-12 17:33_

Weird that this changed

---

_@zanieb reviewed on 2024-02-12 17:34_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_install_scenarios.rs`:894 on 2024-02-12 17:34_

Why did the derivation order change?

---

_@zanieb reviewed on 2024-02-12 17:35_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:47 on 2024-02-12 17:35_

Lots of noise from hash changes here because I changed how hashes are computed.

---

_Review comment by @zanieb on `crates/puffin/tests/pip_install_scenarios.rs`:2625 on 2024-02-12 17:35_

Here begins the meat of the yanked version tests

---

_@zanieb reviewed on 2024-02-12 17:35_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_install_scenarios.rs`:2658 on 2024-02-12 17:38_

This error message can clearly be improved

---

_@zanieb reviewed on 2024-02-12 17:38_

---

_@zanieb reviewed on 2024-02-12 18:27_

---

_Review comment by @zanieb on `scripts/scenarios/update.py`:168 on 2024-02-12 18:27_

https://github.com/astral-sh/puffin/pull/1284

---

_Merged by @zanieb on 2024-02-12 18:44_

---

_Closed by @zanieb on 2024-02-12 18:44_

---

_Branch deleted on 2024-02-12 18:45_

---

_Label `testing` added by @zanieb on 2024-02-12 19:17_

---
