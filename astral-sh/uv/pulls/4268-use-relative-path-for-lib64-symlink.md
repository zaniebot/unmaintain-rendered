```yaml
number: 4268
title: Use relative path for lib64 symlink
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/sym
created_at: 2024-06-12T13:19:46Z
updated_at: 2024-06-12T13:36:28Z
url: https://github.com/astral-sh/uv/pull/4268
synced_at: 2026-01-10T13:54:02Z
```

# Use relative path for lib64 symlink

---

_Pull request opened by @charliermarsh on 2024-06-12 13:19_

## Summary

Closes https://github.com/astral-sh/uv/issues/4265.

## Test Plan

```
❯ ls -l .venv
total 16
-rw-r--r--   1 crmarsh  staff   43 Jun 12 09:23 CACHEDIR.TAG
drwxr-xr-x  14 crmarsh  staff  448 Jun 12 09:23 bin
drwxr-xr-x   3 crmarsh  staff   96 Jun 12 09:23 lib
lrwxr-xr-x   1 crmarsh  staff    3 Jun 12 09:23 lib64 -> lib
-rw-r--r--   1 crmarsh  staff  174 Jun 12 09:23 pyvenv.cfg
```

```
❯ ls .venv/lib64/
python3.12
```


---

_Label `compatibility` added by @charliermarsh on 2024-06-12 13:23_

---

_@zanieb approved on 2024-06-12 13:34_

---

_Merged by @charliermarsh on 2024-06-12 13:36_

---

_Closed by @charliermarsh on 2024-06-12 13:36_

---

_Branch deleted on 2024-06-12 13:36_

---
