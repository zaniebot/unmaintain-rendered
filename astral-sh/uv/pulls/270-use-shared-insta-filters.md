```yaml
number: 270
title: Use shared insta filters
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: use-shared-insta-filters
created_at: 2023-11-01T13:25:42Z
updated_at: 2023-11-02T15:43:00Z
url: https://github.com/astral-sh/uv/pull/270
synced_at: 2026-01-10T15:50:28Z
```

# Use shared insta filters

---

_Pull request opened by @konstin on 2023-11-01 13:25_

Internal refactoring for consistency between tests

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/common/mod.rs`:8 on 2023-11-01 13:31_

Some of these are also used in `pip_sync`...

---

_@charliermarsh reviewed on 2023-11-01 13:31_

---

_@charliermarsh reviewed on 2023-11-01 13:31_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/common/mod.rs`:1 on 2023-11-01 13:31_

Can we leverage this in all tests, not just `pip_compile`?

---

_@konstin reviewed on 2023-11-01 13:41_

---

_Review comment by @konstin on `crates/puffin-cli/tests/common/mod.rs`:8 on 2023-11-01 13:41_

Is that are problem / what do you want instead?

---

_@charliermarsh reviewed on 2023-11-01 13:45_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/common/mod.rs`:8 on 2023-11-01 13:45_

Since these filters are also duplicated in `pip_sync.rs`, we should use the same shared filters there too. The problem is that the filters encode names like `pip-compile`. So those will be unnecessarily included in both cases. Which is weird, but fine.

---

_@charliermarsh approved on 2023-11-02 15:03_

---

_Merged by @konstin on 2023-11-02 15:42_

---

_Closed by @konstin on 2023-11-02 15:42_

---

_Branch deleted on 2023-11-02 15:43_

---
