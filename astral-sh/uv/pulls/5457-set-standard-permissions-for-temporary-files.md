```yaml
number: 5457
title: Set standard permissions for temporary files
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/temp-perms
created_at: 2024-07-25T18:24:39Z
updated_at: 2024-07-25T20:50:31Z
url: https://github.com/astral-sh/uv/pull/5457
synced_at: 2026-01-12T16:06:49Z
```

# Set standard permissions for temporary files

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5435.

## Test Plan

Before:

```
❯ ls -l .venv/lib/python3.12/site-packages/httpx-0.27.0.dist-info
total 48
-rw-------  1 crmarsh  staff     2 Jul 25 14:21 INSTALLER
-rw-r--r--  1 crmarsh  staff  7184 Jul 23 23:20 METADATA
-rw-r--r--  1 crmarsh  staff  2541 Jul 25 14:21 RECORD
-rw-------  1 crmarsh  staff     0 Jul 25 14:21 REQUESTED
-rw-r--r--  1 crmarsh  staff    87 Jul 23 23:20 WHEEL
-rw-r--r--  1 crmarsh  staff    37 Jul 23 23:20 entry_points.txt
drwxr-xr-x  3 crmarsh  staff    96 Jul 25 14:21 licenses
```

After:

```
❯ ls -l .venv/lib/python3.12/site-packages/flask-3.0.3.dist-info/
total 48
-rw-r--r--  1 crmarsh  staff     2 Jul 25 14:21 INSTALLER
-rw-r--r--  1 crmarsh  staff  1475 Jul 25 14:21 LICENSE.txt
-rw-r--r--  1 crmarsh  staff  3177 Jul 25 14:21 METADATA
-rw-r--r--  1 crmarsh  staff  2565 Jul 25 14:21 RECORD
-rw-r--r--  1 crmarsh  staff     0 Jul 25 14:21 REQUESTED
-rw-r--r--  1 crmarsh  staff    81 Jul 25 14:21 WHEEL
-rw-r--r--  1 crmarsh  staff    40 Jul 25 14:21 entry_points.txt
```


---

_Label `bug` added by @charliermarsh on 2024-07-25 18:24_

---

_Marked ready for review by @charliermarsh on 2024-07-25 18:24_

---

_@zanieb approved on 2024-07-25 18:59_

---

_@zanieb reviewed on 2024-07-25 18:59_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:104 on 2024-07-25 18:59_

Should the doc note that it handles permissions?

---

_Merged by @charliermarsh on 2024-07-25 20:50_

---

_Closed by @charliermarsh on 2024-07-25 20:50_

---

_Branch deleted on 2024-07-25 20:50_

---
