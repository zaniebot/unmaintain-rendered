```yaml
number: 3015
title: Log hardlink failures
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/improve-hardlink-failure-logging
created_at: 2024-04-12T14:54:52Z
updated_at: 2024-04-12T15:06:39Z
url: https://github.com/astral-sh/uv/pull/3015
synced_at: 2026-01-12T16:05:22Z
```

# Log hardlink failures

---

_@konstin_

Inspired by https://github.com/astral-sh/uv/issues/2964, we now properly log hardlink failures, e.g. when the cache is a docker container but the venv is in a bind mount, e.g.:

```
DEBUG Failed to hardlink `/code/venv/uv/lib/python3.12/site-packages/asgiref-3.8.1.dist-info/WHEEL` to `/root/.cache/uv/archive-v0/nnpkKgUoM3LMxcNDmEKJQ/asgiref-3.8.1.dist-info/WHEEL`, attempting to copy files as a fallback
```


---

_Label `enhancement` added by @konstin on 2024-04-12 14:54_

---

_@charliermarsh approved on 2024-04-12 15:01_

---

_Merged by @konstin on 2024-04-12 15:06_

---

_Closed by @konstin on 2024-04-12 15:06_

---

_Branch deleted on 2024-04-12 15:06_

---
