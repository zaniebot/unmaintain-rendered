```yaml
number: 9280
title: "Parallelize network requests in `uv tree --outdated`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/par
created_at: 2024-11-20T16:24:06Z
updated_at: 2024-11-20T16:45:15Z
url: https://github.com/astral-sh/uv/pull/9280
synced_at: 2026-01-10T12:00:00Z
```

# Parallelize network requests in `uv tree --outdated`

---

_Pull request opened by @charliermarsh on 2024-11-20 16:24_

## Summary

Closes https://github.com/astral-sh/uv/issues/9266.


---

_Comment by @charliermarsh on 2024-11-20 16:29_

Separately, I'll probably add a progress bar for this.

---

_Label `performance` added by @charliermarsh on 2024-11-20 16:29_

---

_@konstin approved on 2024-11-20 16:29_

That's much faster!

![image](https://github.com/user-attachments/assets/8a312810-d834-43ef-99c2-400836243002)


---

_Comment by @charliermarsh on 2024-11-20 16:34_

While I was here, I also modified `uv tree --outdated` such that we no longer drop network errors.

---

_Merged by @charliermarsh on 2024-11-20 16:45_

---

_Closed by @charliermarsh on 2024-11-20 16:45_

---

_Branch deleted on 2024-11-20 16:45_

---
