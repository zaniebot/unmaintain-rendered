```yaml
number: 7773
title: Use file stem when parsing cached wheel names
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: charlie/http
created_at: 2024-09-29T01:46:33Z
updated_at: 2024-09-29T16:05:17Z
url: https://github.com/astral-sh/uv/pull/7773
synced_at: 2026-01-10T12:53:55Z
```

# Use file stem when parsing cached wheel names

---

_Pull request opened by @charliermarsh on 2024-09-29 01:46_

## Summary

I noticed that we were including `http` (the file extension) in the platform tags when reading from the cache:

![Screenshot 2024-09-28 at 9 40 15 PM](https://github.com/user-attachments/assets/d80ed351-1257-42b5-8292-0b11a50c767d)

Probably harmless, but wrong.


---

_Marked ready for review by @charliermarsh on 2024-09-29 01:46_

---

_Label `bug` added by @charliermarsh on 2024-09-29 01:46_

---

_Label `cache` added by @charliermarsh on 2024-09-29 01:46_

---

_@zanieb approved on 2024-09-29 15:31_

---

_Merged by @charliermarsh on 2024-09-29 16:05_

---

_Closed by @charliermarsh on 2024-09-29 16:05_

---

_Branch deleted on 2024-09-29 16:05_

---
