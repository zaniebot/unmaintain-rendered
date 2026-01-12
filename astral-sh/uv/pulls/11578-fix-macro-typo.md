```yaml
number: 11578
title: Fix macro typo
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/fix-macro-typo
created_at: 2025-02-17T15:22:45Z
updated_at: 2025-02-17T15:57:54Z
url: https://github.com/astral-sh/uv/pull/11578
synced_at: 2026-01-12T16:09:53Z
```

# Fix macro typo

---

_@jtfmumm_

This typo wasn't caught because the `($arg:expr, false)` macro branch was never exercised.

For example, prior to this change, if you add
```
show_settings!(globals, false);
```
below, you'll get a compiler error.


---

_@charliermarsh approved on 2025-02-17 15:24_

We could also just remove that branch -- either way. (I believe this is just a repeat of a definition elsewhere in this file, so I assume it's there for parity.)

---

_Label `internal` added by @zanieb on 2025-02-17 15:24_

---

_Comment by @jtfmumm on 2025-02-17 15:47_

> We could also just remove that branch -- either way. (I believe this is just a repeat of a definition elsewhere in this file, so I assume it's there for parity.)

Makes sense. I'll just remove the branch.

---

_Merged by @jtfmumm on 2025-02-17 15:57_

---

_Closed by @jtfmumm on 2025-02-17 15:57_

---

_Branch deleted on 2025-02-17 15:57_

---
