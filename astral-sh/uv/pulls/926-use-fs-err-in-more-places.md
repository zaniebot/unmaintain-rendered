```yaml
number: 926
title: Use fs_err in more places
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/more-fs_err
created_at: 2024-01-15T09:37:37Z
updated_at: 2024-01-15T09:39:34Z
url: https://github.com/astral-sh/uv/pull/926
synced_at: 2026-01-12T16:04:18Z
```

# Use fs_err in more places

---

_@konstin_

Before:

```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: jaxlib==0.4.23+cuda12.cudnn89
  Caused by: Directory not empty (os error 39)
```

After:

```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: jaxlib==0.4.23+cuda12.cudnn89
  Caused by: failed to rename file from /home/konsti/.cache/puffin/.tmpcG7tVP/jaxlib-0.4.23+cuda12.cudnn89-cp310-cp310-manylinux2014_x86_64.whl to /home/konsti/.cache/puffin/wheels-v0/index/9ff50b883297fa9d/jaxlib/jaxlib-0.4.23+cuda12.cudnn89-cp310-cp310-manylinux2014_x86_64
  Caused by: Directory not empty (os error 39)
```

---

_Label `error messages` added by @konstin on 2024-01-15 09:37_

---

_Merged by @konstin on 2024-01-15 09:39_

---

_Closed by @konstin on 2024-01-15 09:39_

---

_Branch deleted on 2024-01-15 09:39_

---
