```yaml
number: 1187
title: Abstract away test duplication in pip-compile
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/test-slimming
created_at: 2024-01-30T13:31:01Z
updated_at: 2024-01-31T16:11:11Z
url: https://github.com/astral-sh/uv/pull/1187
synced_at: 2026-01-10T15:39:03Z
```

# Abstract away test duplication in pip-compile

---

_Pull request opened by @konstin on 2024-01-30 13:31_

In preparation for the new windows handling, i want to introduce a `TestContext` and `puffin_snapshot!` abstraction. This PR applies those changes for pip-compile. My plan is to use those for all venv-based integration tests and build the custom windows filters on top of  `puffin_snapshot!`.

---

_Review requested from @charliermarsh by @konstin on 2024-01-30 13:31_

---

_Review requested from @zanieb by @konstin on 2024-01-30 13:31_

---

_@zanieb reviewed on 2024-01-30 16:15_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile.rs`:51 on 2024-01-30 16:15_

Should the exclude newer date be in the snapshot?

---

_@konstin reviewed on 2024-01-30 16:16_

---

_Review comment by @konstin on `crates/puffin/tests/pip_compile.rs`:51 on 2024-01-30 16:16_

It helpful for reproducing i guess

---

_@zanieb approved on 2024-01-30 16:53_

Seems like a good start

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile.rs`:25 on 2024-01-30 17:42_

Can we derive `Debug` on this for the future?

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile.rs`:34 on 2024-01-30 17:42_

Nit: add a real message to `expect`? Or use `unwrap`.

---

_@charliermarsh approved on 2024-01-30 17:43_

---

_Merged by @konstin on 2024-01-31 16:11_

---

_Closed by @konstin on 2024-01-31 16:11_

---

_Branch deleted on 2024-01-31 16:11_

---
