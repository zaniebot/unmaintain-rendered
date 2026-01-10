```yaml
number: 1300
title: Reset all of the snapshots for consistent indentation
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/snapshots
created_at: 2024-02-14T18:43:45Z
updated_at: 2024-02-14T18:50:29Z
url: https://github.com/astral-sh/uv/pull/1300
synced_at: 2026-01-10T15:33:24Z
```

# Reset all of the snapshots for consistent indentation

---

_Pull request opened by @zanieb on 2024-02-14 18:43_

This is really annoying, but the snapshots keep changing indentation when updated.

I could not get insta to update them. So I added a print statement to `main` and  updated the snapshots, then removed the statement and updated the snapshots again to force them all to refresh.



---

_Label `internal` added by @zanieb on 2024-02-14 18:43_

---

_Comment by @zanieb on 2024-02-14 18:48_

Ref https://github.com/astral-sh/puffin/pull/1298#discussion_r1489853384

---

_@charliermarsh approved on 2024-02-14 18:49_

---

_Merged by @zanieb on 2024-02-14 18:50_

---

_Closed by @zanieb on 2024-02-14 18:50_

---

_Branch deleted on 2024-02-14 18:50_

---
