```yaml
number: 7421
title: Fix source distribution filename escaping
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/dist-info-underscore
created_at: 2024-09-16T10:19:11Z
updated_at: 2024-09-16T10:39:08Z
url: https://github.com/astral-sh/uv/pull/7421
synced_at: 2026-01-12T16:07:49Z
```

# Fix source distribution filename escaping

---

_@konstin_

The distribution name in source distribution filenames is escaped with underscores instead of dashes, so that we can split at the dash after the name.

See https://packaging.python.org/en/latest/specifications/source-distribution-format/#source-distribution-file-name and https://packaging.python.org/en/latest/specifications/binary-distribution-format/#escaping-and-unicode

---

_Label `bug` added by @konstin on 2024-09-16 10:19_

---

_Merged by @konstin on 2024-09-16 10:25_

---

_Closed by @konstin on 2024-09-16 10:25_

---

_Branch deleted on 2024-09-16 10:25_

---

_Renamed from "Fix sourc distribution filename escaping" to "Fix source distribution filename escaping" by @konstin on 2024-09-16 10:39_

---
