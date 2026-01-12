```yaml
number: 10352
title: "Add `Send + 'static` bounds to cache deserialization"
type: pull_request
state: merged
author: konstin
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: konsti/send-deserialization
created_at: 2025-01-07T09:45:47Z
updated_at: 2025-01-07T09:55:38Z
url: https://github.com/astral-sh/uv/pull/10352
synced_at: 2026-01-12T16:09:15Z
```

# Add `Send + 'static` bounds to cache deserialization

---

_@konstin_

Ref https://github.com/astral-sh/uv/issues/10344

Another failed optimization, but I'm adding those bounds still since they are already fulfilled and allow moving deserialization to a thread once the resolver thread is fast enough for this to matter.

---

_Label `performance` added by @konstin on 2025-01-07 09:45_

---

_Label `internal` added by @konstin on 2025-01-07 09:45_

---

_Merged by @konstin on 2025-01-07 09:55_

---

_Closed by @konstin on 2025-01-07 09:55_

---

_Branch deleted on 2025-01-07 09:55_

---
