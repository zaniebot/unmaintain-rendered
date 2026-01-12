```yaml
number: 3539
title: Cleaning an empty cache removes 4 files
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-05-13T08:32:44Z
updated_at: 2024-05-15T16:29:40Z
url: https://github.com/astral-sh/uv/issues/3539
synced_at: 2026-01-12T15:58:44Z
```

# Cleaning an empty cache removes 4 files

---

_@konstin_

Repeatedly calling `uv cache clean` removes 4 files every time on ubuntu 24.04. This should not happen, `uv cache clean` shouldn't create a cache directory before cleaning (and/or we should count correctly)

```
Clearing cache at: /home/konsti/.cache/uv
Removed 4 files (44B)
```

---

_Label `bug` added by @konstin on 2024-05-13 08:32_

---

_Comment by @charliermarsh on 2024-05-13 14:00_

Yeah I looked at this recently too. The problem is that we initialize the cache in `main.rs` every time IIRC. And that involves creating a few files.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-15 16:14_

---

_Closed by @charliermarsh on 2024-05-15 16:29_

---
