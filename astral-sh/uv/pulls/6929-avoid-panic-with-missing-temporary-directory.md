```yaml
number: 6929
title: Avoid panic with missing temporary directory
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/missing-temp
created_at: 2024-09-02T07:26:21Z
updated_at: 2024-09-02T07:32:43Z
url: https://github.com/astral-sh/uv/pull/6929
synced_at: 2026-01-12T16:07:36Z
```

# Avoid panic with missing temporary directory

---

_@konstin_

Forward an error for missing temp directories:

```
$ env TMPDIR=.tmp uv-debug pip install httpx
  error: No such file or directory (os error 2) at path "/home/konsti/projects/uv/.tmp/.tmpgIBhhh"
```

Fixes #6878

---

_Label `bug` added by @konstin on 2024-09-02 07:26_

---

_Merged by @konstin on 2024-09-02 07:32_

---

_Closed by @konstin on 2024-09-02 07:32_

---

_Branch deleted on 2024-09-02 07:32_

---
