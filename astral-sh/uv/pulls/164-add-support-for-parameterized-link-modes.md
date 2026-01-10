```yaml
number: 164
title: Add support for parameterized link modes
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/link-mode
created_at: 2023-10-22T04:13:57Z
updated_at: 2023-10-23T13:06:29Z
url: https://github.com/astral-sh/uv/pull/164
synced_at: 2026-01-10T15:50:28Z
```

# Add support for parameterized link modes

---

_Pull request opened by @charliermarsh on 2023-10-22 04:13_

Allows the user to select between clone, hardlink, and copy semantics for installs. (The pnpm documentation has a decent description of what these mean: https://pnpm.io/npmrc#package-import-method.)

Closes #159.


---

_Merged by @charliermarsh on 2023-10-22 04:35_

---

_Closed by @charliermarsh on 2023-10-22 04:35_

---

_Branch deleted on 2023-10-22 04:35_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/linker.rs`:250 on 2023-10-23 07:37_

What does this do on non-reflink platforms?

---

_@konstin reviewed on 2023-10-23 07:37_

---

_@konstin reviewed on 2023-10-23 07:39_

---

_Review comment by @konstin on `crates/puffin-cli/tests/snapshots/pip_sync__install.snap`:9 on 2023-10-23 07:39_

Should that path be filtered out?

---

_@charliermarsh reviewed on 2023-10-23 13:05_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:250 on 2023-10-23 13:05_

Error!

---

_@charliermarsh reviewed on 2023-10-23 13:06_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/snapshots/pip_sync__install.snap`:9 on 2023-10-23 13:06_

Oh, maybe... But the args aren't compared in the snapshot.

---
