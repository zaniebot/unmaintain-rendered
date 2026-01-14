```yaml
number: 17452
title: "Only save Rust cache on `main` or when cache-relevant files change"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/cache-smart-save-if
created_at: 2026-01-13T22:51:04Z
updated_at: 2026-01-14T13:35:48Z
url: https://github.com/astral-sh/uv/pull/17452
synced_at: 2026-01-14T14:41:38Z
```

# Only save Rust cache on `main` or when cache-relevant files change

---

_@zanieb_

The goal here is to reduce cache consumption and contention by avoiding cache saves on pull requests unless the pull request makes a change that requires a new cache entry.

---

_Renamed from "Only save Rust cache when cache-relevant files change" to "Only save Rust cache on `main` or when cache-relevant files change" by @zanieb on 2026-01-13 22:52_

---

_Marked ready for review by @zanieb on 2026-01-13 23:20_

---

_Label `internal` added by @zanieb on 2026-01-13 23:20_

---

_@konstin approved on 2026-01-14 09:20_

---

_Merged by @zanieb on 2026-01-14 13:35_

---

_Closed by @zanieb on 2026-01-14 13:35_

---

_Branch deleted on 2026-01-14 13:35_

---
