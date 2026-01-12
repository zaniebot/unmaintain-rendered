```yaml
number: 4544
title: "Remove `InMemoryIndexRef`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/remove-in-memory-index-ref
created_at: 2024-06-26T12:37:01Z
updated_at: 2024-06-26T13:12:26Z
url: https://github.com/astral-sh/uv/pull/4544
synced_at: 2026-01-12T16:06:18Z
```

# Remove `InMemoryIndexRef`

---

_@konstin_

`InMemoryIndex` has recently been turned into an `Arc`, so we can now freely copy it instead using `Cow` tricks.

---

_Label `internal` added by @konstin on 2024-06-26 12:37_

---

_Review requested from @charliermarsh by @konstin on 2024-06-26 12:37_

---

_@BurntSushi approved on 2024-06-26 12:38_

---

_@charliermarsh approved on 2024-06-26 13:11_

---

_Merged by @charliermarsh on 2024-06-26 13:11_

---

_Closed by @charliermarsh on 2024-06-26 13:11_

---

_Branch deleted on 2024-06-26 13:11_

---

_Comment by @charliermarsh on 2024-06-26 13:11_

Just confirming that cloning here shares the same underlying data.

---

_Comment by @konstin on 2024-06-26 13:12_

Yep, the `Arc` clone refers to the same data

---
