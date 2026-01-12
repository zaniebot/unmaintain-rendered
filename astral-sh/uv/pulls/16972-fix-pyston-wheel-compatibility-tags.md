```yaml
number: 16972
title: Fix Pyston wheel compatibility tags
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-pyston-tag
created_at: 2025-12-03T16:38:28Z
updated_at: 2025-12-04T09:18:55Z
url: https://github.com/astral-sh/uv/pull/16972
synced_at: 2026-01-12T16:12:32Z
```

# Fix Pyston wheel compatibility tags

---

_@konstin_

This was discovered by https://github.com/astral-sh/uv/pull/16074, where the wrong tag now fails the Pyston integration test.

I'm not sure what the exact schema of the cache tag is, but since the project is dead, I don't expect any new non-matching versions to follow.

---

_Label `bug` added by @konstin on 2025-12-03 16:38_

---

_@zanieb approved on 2025-12-03 17:51_

---

_Merged by @konstin on 2025-12-04 09:18_

---

_Closed by @konstin on 2025-12-04 09:18_

---

_Branch deleted on 2025-12-04 09:18_

---

_Renamed from "Fix Pyston tags" to "Fix Pyston wheel compatibility tags" by @konstin on 2025-12-04 09:18_

---
