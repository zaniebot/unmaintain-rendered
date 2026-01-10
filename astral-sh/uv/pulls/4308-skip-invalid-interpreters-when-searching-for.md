```yaml
number: 4308
title: Skip invalid interpreters when searching for requested interpreter executable name
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/executable-name-find
created_at: 2024-06-13T16:35:41Z
updated_at: 2024-06-13T17:36:13Z
url: https://github.com/astral-sh/uv/pull/4308
synced_at: 2026-01-10T13:54:02Z
```

# Skip invalid interpreters when searching for requested interpreter executable name

---

_Pull request opened by @zanieb on 2024-06-13 16:35_

Previously, we took the first executable on the `PATH` but if it was not a usable interpreter we'd fail. Now, we'll continue searching in the path until we find an interpreter as we do with the standard executable names.

---

_Label `enhancement` added by @zanieb on 2024-06-13 16:35_

---

_@zanieb reviewed on 2024-06-13 16:40_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:365 on 2024-06-13 16:40_

This refactor wasn't needed for this pull request but we'll do this in the next change to facilitate respecting the system python preferences when reading executable names.

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:516 on 2024-06-13 16:41_

This is the core change

---

_@zanieb reviewed on 2024-06-13 16:41_

---

_Marked ready for review by @zanieb on 2024-06-13 17:03_

---

_Renamed from "Search for valid interpreter with requested executable name" to "Skip invalid interpreters when searching for requested interpreter executable name" by @zanieb on 2024-06-13 17:04_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-13 17:09_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-13 17:09_

---

_@zanieb reviewed on 2024-06-13 17:19_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:365 on 2024-06-13 17:19_

TODO addressed in https://github.com/astral-sh/uv/pull/4310

---

_@BurntSushi approved on 2024-06-13 17:31_

---

_Merged by @zanieb on 2024-06-13 17:36_

---

_Closed by @zanieb on 2024-06-13 17:36_

---

_Branch deleted on 2024-06-13 17:36_

---
