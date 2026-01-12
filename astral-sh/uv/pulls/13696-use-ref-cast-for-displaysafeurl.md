```yaml
number: 13696
title: "Use ref-cast for `DisplaySafeUrl`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/use-ref-cast-for-url
created_at: 2025-05-28T08:41:50Z
updated_at: 2025-05-28T11:28:29Z
url: https://github.com/astral-sh/uv/pull/13696
synced_at: 2026-01-12T16:10:48Z
```

# Use ref-cast for `DisplaySafeUrl`

---

_@konstin_

By default, Rust does not support safe cast from `&U` to `&T` for `#[repr(transparent)] T(U)` even if the newtype opts in. The dtolnay ref-cast crate fills this gap, allowing to remove `DisplaySafeUrlRef`.

---

_Review requested from @jtfmumm by @konstin on 2025-05-28 08:41_

---

_Label `internal` added by @konstin on 2025-05-28 08:41_

---

_@jtfmumm approved on 2025-05-28 10:11_

This is nice!

---

_Merged by @konstin on 2025-05-28 11:28_

---

_Closed by @konstin on 2025-05-28 11:28_

---

_Branch deleted on 2025-05-28 11:28_

---
