```yaml
number: 268
title: Only fall back to copy when the first hard linking failed
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: hard-link-fallback
created_at: 2023-11-01T13:06:29Z
updated_at: 2023-11-01T17:35:53Z
url: https://github.com/astral-sh/uv/pull/268
synced_at: 2026-01-10T15:50:28Z
```

# Only fall back to copy when the first hard linking failed

---

_Pull request opened by @konstin on 2023-11-01 13:06_

Hard linking might not be supported but we (afaik) can't detect this ahead of time, so we'll try hard linking the first file, if this succeeds we'll know later hard linking errors are not due to lack of os/fs support, if it fails we'll switch to copying for the rest of the install. Follow up to https://github.com/astral-sh/puffin/pull/237#discussion_r1376705137

---

_Marked ready for review by @konstin on 2023-11-01 13:17_

---

_@charliermarsh reviewed on 2023-11-01 13:22_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:390 on 2023-11-01 13:22_

Don't we need to raise the error if it's not the first try?

---

_@konstin reviewed on 2023-11-01 13:27_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/linker.rs`:390 on 2023-11-01 13:27_

yes!

---

_@charliermarsh approved on 2023-11-01 13:28_

:D

---

_Merged by @konstin on 2023-11-01 17:35_

---

_Closed by @konstin on 2023-11-01 17:35_

---

_Branch deleted on 2023-11-01 17:35_

---
