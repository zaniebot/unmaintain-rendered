```yaml
number: 12466
title: Use 64-bit host for 32-bit releases to work around OOM
type: pull_request
state: merged
author: konstin
labels:
  - releases
assignees: []
merged: true
base: main
head: konsti/fix-i686-oom
created_at: 2025-03-25T14:44:20Z
updated_at: 2025-03-25T17:33:32Z
url: https://github.com/astral-sh/uv/pull/12466
synced_at: 2026-01-10T11:10:40Z
```

# Use 64-bit host for 32-bit releases to work around OOM

---

_Pull request opened by @konstin on 2025-03-25 14:44_

The i686 linux gnu release job started failing since the last release (#12430) due to an OOM with llvm breaking the 4GB limit for 32-bit processes. We work around this by using a 64-bit host targeting 32-bit.

---

_Label `releases` added by @konstin on 2025-03-25 14:44_

---

_@zanieb approved on 2025-03-25 17:24_

---

_Review requested from @geofft by @konstin on 2025-03-25 17:33_

---

_Merged by @konstin on 2025-03-25 17:33_

---

_Closed by @konstin on 2025-03-25 17:33_

---

_Branch deleted on 2025-03-25 17:33_

---
