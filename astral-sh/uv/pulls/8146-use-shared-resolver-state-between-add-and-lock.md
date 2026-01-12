```yaml
number: 8146
title: Use shared resolver state between add and lock
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-10-12T14:19:11Z
updated_at: 2024-10-12T14:58:08Z
url: https://github.com/astral-sh/uv/pull/8146
synced_at: 2026-01-12T16:08:10Z
```

# Use shared resolver state between add and lock

---

_@charliermarsh_

## Summary

If you `uv add` a Git dependency, we resolve it twice:

![Screenshot 2024-10-12 at 2 17 27 PM](https://github.com/user-attachments/assets/342e2523-af06-4783-b836-93b6bd9f34bc)

While we need to avoid sharing state between `lock` and `sync` (see the large TODO that moved in this change), we should prioritize sharing state between different resolver operations.


---

_Label `bug` added by @charliermarsh on 2024-10-12 14:19_

---

_Merged by @charliermarsh on 2024-10-12 14:58_

---

_Closed by @charliermarsh on 2024-10-12 14:58_

---

_Branch deleted on 2024-10-12 14:58_

---
