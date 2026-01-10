```yaml
number: 3234
title: Fix single crate tokio features
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-single-crate-tokio-compilation
created_at: 2024-04-24T08:44:49Z
updated_at: 2024-04-24T08:55:16Z
url: https://github.com/astral-sh/uv/pull/3234
synced_at: 2026-01-10T14:37:54Z
```

# Fix single crate tokio features

---

_Pull request opened by @konstin on 2024-04-24 08:44_

Previously, uv-auth would fail to compile due to a missing process feature. I chose to make all tokio features we use top level features, so we can share the tokio cache between all test invocations.

---

_Label `internal` added by @konstin on 2024-04-24 08:44_

---

_Merged by @konstin on 2024-04-24 08:55_

---

_Closed by @konstin on 2024-04-24 08:55_

---

_Branch deleted on 2024-04-24 08:55_

---
