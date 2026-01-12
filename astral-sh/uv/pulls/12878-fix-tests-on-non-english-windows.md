```yaml
number: 12878
title: Fix tests on non-English Windows
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - windows
assignees: []
merged: true
base: main
head: konsti/testing-on-windows
created_at: 2025-04-14T11:28:00Z
updated_at: 2025-04-14T13:50:57Z
url: https://github.com/astral-sh/uv/pull/12878
synced_at: 2026-01-12T16:10:25Z
```

# Fix tests on non-English Windows

---

_@konstin_

By default, unlike on CI, a Windows machine does not allow creating symlinks, so we have to unix-gate tests that assume symlinks.

We can't install the transformers ecosystem test on Windows due to missing torch, so it is also unix-gated.

Windows translates error messages, so we have to filter the "File not found" message, since it can also be a "Datei nicht gefunden".

---

_Label `internal` added by @konstin on 2025-04-14 11:28_

---

_Label `windows` added by @konstin on 2025-04-14 11:28_

---

_Review requested from @Gankra by @konstin on 2025-04-14 11:28_

---

_@Gankra approved on 2025-04-14 13:49_

---

_Merged by @konstin on 2025-04-14 13:50_

---

_Closed by @konstin on 2025-04-14 13:50_

---

_Branch deleted on 2025-04-14 13:50_

---
