```yaml
number: 394
title: Delete any directories listed in the RECORD file
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dir
created_at: 2023-11-10T17:35:55Z
updated_at: 2023-11-10T18:17:53Z
url: https://github.com/astral-sh/uv/pull/394
synced_at: 2026-01-12T16:03:55Z
```

# Delete any directories listed in the RECORD file

---

_@charliermarsh_

## Summary

It looks like, when you install `pip`, it includes a bunch of `__pycache__` directories in the RECORD file (although these directories don't exist until you run `pip`). Our uninstaller assumed that the RECORD file only contained _files_.

Closes https://github.com/astral-sh/puffin/issues/389.


---

_Review requested from @konstin by @charliermarsh on 2023-11-10 17:35_

---

_Label `bug` added by @charliermarsh on 2023-11-10 17:36_

---

_Comment by @charliermarsh on 2023-11-10 18:14_

Small change so just gonna merge to update tests too.

---

_Merged by @charliermarsh on 2023-11-10 18:17_

---

_Closed by @charliermarsh on 2023-11-10 18:17_

---

_Branch deleted on 2023-11-10 18:17_

---
