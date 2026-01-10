```yaml
number: 15418
title: Stop leaking strings in Python downloads
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/stop-leaking-strings
created_at: 2025-08-21T14:23:23Z
updated_at: 2025-08-21T15:54:40Z
url: https://github.com/astral-sh/uv/pull/15418
synced_at: 2026-01-10T06:44:33Z
```

# Stop leaking strings in Python downloads

---

_Pull request opened by @konstin on 2025-08-21 14:23_

We should not unnecessarily leak memory. Instead, we follow the general patterns and use `Cow` for strings that can be from either a static or a dynamic source.

---

_Review requested from @zanieb by @konstin on 2025-08-21 14:23_

---

_Label `internal` added by @konstin on 2025-08-21 14:23_

---

_@konstin reviewed on 2025-08-21 14:23_

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:331 on 2025-08-21 14:23_

This only allocates if we have a `Cow::Owned`

---

_@konstin reviewed on 2025-08-21 14:24_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:839 on 2025-08-21 14:24_

Returning the full `Cow` here allows the cheap clones below.

---

_Comment by @zanieb on 2025-08-21 15:11_

cc @Gankra as the reviewer on https://github.com/astral-sh/uv/pull/10939

---

_@zanieb approved on 2025-08-21 15:52_

---

_Merged by @konstin on 2025-08-21 15:54_

---

_Closed by @konstin on 2025-08-21 15:54_

---

_Branch deleted on 2025-08-21 15:54_

---
