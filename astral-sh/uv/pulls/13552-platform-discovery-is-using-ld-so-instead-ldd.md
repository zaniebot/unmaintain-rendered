```yaml
number: 13552
title: Platform discovery is using ld.so instead ldd
type: pull_request
state: merged
author: konstin
labels:
  - tracing
assignees: []
merged: true
base: main
head: konsti/ld-os-in-stead-of-ldd
created_at: 2025-05-20T10:24:03Z
updated_at: 2025-05-20T13:27:24Z
url: https://github.com/astral-sh/uv/pull/13552
synced_at: 2026-01-10T11:10:41Z
```

# Platform discovery is using ld.so instead ldd

---

_Pull request opened by @konstin on 2025-05-20 10:24_

In platform discovery we're parsing the output of the ELF interpreter, e.g., `/lib64/ld-linux-x86-64.so.2`. This file is ld, not ldd, which was incorrectly named in the code.

An alternative is naming everything ELF interpreter instead of ld.so.

---

_Review requested from @geofft by @konstin on 2025-05-20 10:24_

---

_Review requested from @oconnor663 by @konstin on 2025-05-20 10:24_

---

_Label `internal` added by @konstin on 2025-05-20 10:24_

---

_Label `internal` removed by @konstin on 2025-05-20 10:24_

---

_Label `tracing` added by @konstin on 2025-05-20 10:24_

---

_@zanieb approved on 2025-05-20 13:10_

---

_Merged by @konstin on 2025-05-20 13:27_

---

_Closed by @konstin on 2025-05-20 13:27_

---

_Branch deleted on 2025-05-20 13:27_

---
