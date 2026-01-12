```yaml
number: 15729
title: "Windows CI flake: not enough disk space"
type: issue
state: closed
author: konstin
labels:
  - ci-flake
assignees: []
created_at: 2025-09-08T06:56:20Z
updated_at: 2025-09-08T13:21:40Z
url: https://github.com/astral-sh/uv/issues/15729
synced_at: 2026-01-12T16:02:16Z
```

# Windows CI flake: not enough disk space

---

_@konstin_

E.g. https://github.com/astral-sh/uv/actions/runs/17537693772/job/49803727470?pr=15724

```
error: Failed to install cpython-3.11.13-[PLATFORM]
  Caused by: Failed to extract archive: cpython-3.11.13-[PLATFORM]-windows-msvc-install_only_stripped.tar.gz
  Caused by: I/O operation failed during extraction
  Caused by: failed to create `[TEMP_DIR]/managed/.temp/[TMP]/__pycache__`
  Caused by: There is not enough space on the disk. (os error 112)
```

---

_Label `ci-flake` added by @konstin on 2025-09-08 06:56_

---

_Closed by @zanieb on 2025-09-08 13:21_

---
