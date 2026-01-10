```yaml
number: 14448
title: Fix optional cfg gates
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-optional-import-gates
created_at: 2025-07-03T19:46:42Z
updated_at: 2025-07-03T20:29:05Z
url: https://github.com/astral-sh/uv/pull/14448
synced_at: 2026-01-10T06:53:01Z
```

# Fix optional cfg gates

---

_Pull request opened by @konstin on 2025-07-03 19:46_

Running `cargo clippy` in individual crates could raise warnings due to unused imports as `Cow` is only used with `#[cfg(feature = "schemars")]`

---

_Label `internal` added by @konstin on 2025-07-03 19:46_

---

_@charliermarsh approved on 2025-07-03 20:12_

---

_@zanieb approved on 2025-07-03 20:28_

---

_Merged by @zanieb on 2025-07-03 20:29_

---

_Closed by @zanieb on 2025-07-03 20:29_

---

_Branch deleted on 2025-07-03 20:29_

---
