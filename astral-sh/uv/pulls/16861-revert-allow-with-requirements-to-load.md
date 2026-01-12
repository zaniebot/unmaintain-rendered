```yaml
number: 16861
title: "Revert \"Allow `--with-requirements` to load extensionless inline-metadata scripts\""
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/revert-requirement-sniff
created_at: 2025-11-26T14:44:18Z
updated_at: 2025-11-26T14:57:46Z
url: https://github.com/astral-sh/uv/pull/16861
synced_at: 2026-01-12T16:12:29Z
```

# Revert "Allow `--with-requirements` to load extensionless inline-metadata scripts"

---

_@zanieb_

Reverts https://github.com/astral-sh/uv/pull/16805 / https://github.com/astral-sh/uv/pull/16744

This also invalidates

- https://github.com/astral-sh/uv/pull/16855
- #16857 

There's probably a way we can make this work, but detecting whether a file is safe to read repeatedly is non-trivial, `is_file` returns `true` for `/dev/stdin` on macOS so the approach from #16857 is not sufficient. I spent a while trying to add `is_char_device` detection for macOS but unfortunately that didn't work.

---

_@charliermarsh approved on 2025-11-26 14:44_

---

_Renamed from "Revert "Allow `--with-requirements` to load extensionless inline-metadata scripts (#16805)"" to "Revert "Allow `--with-requirements` to load extensionless inline-metadata scripts"" by @zanieb on 2025-11-26 14:47_

---

_Label `bug` added by @zanieb on 2025-11-26 14:47_

---

_Marked ready for review by @zanieb on 2025-11-26 14:47_

---

_Merged by @zanieb on 2025-11-26 14:57_

---

_Closed by @zanieb on 2025-11-26 14:57_

---

_Branch deleted on 2025-11-26 14:57_

---
