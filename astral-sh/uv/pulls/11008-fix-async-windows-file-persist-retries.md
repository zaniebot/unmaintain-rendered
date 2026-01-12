```yaml
number: 11008
title: fix async windows file persist retries
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/fix-persist
created_at: 2025-01-27T23:11:10Z
updated_at: 2025-01-27T23:51:38Z
url: https://github.com/astral-sh/uv/pull/11008
synced_at: 2026-01-12T16:09:37Z
```

# fix async windows file persist retries

---

_@Gankra_

The previous two versions of the code were bugged and would always produce None when you retried (producing a hard LostState error).

---

_Label `bug` added by @Gankra on 2025-01-27 23:11_

---

_@charliermarsh approved on 2025-01-27 23:23_

---

_Merged by @Gankra on 2025-01-27 23:51_

---

_Closed by @Gankra on 2025-01-27 23:51_

---

_Branch deleted on 2025-01-27 23:51_

---
