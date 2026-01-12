```yaml
number: 67
title: "Use recursive `clonefile` calls on macOS"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/reflink
created_at: 2023-10-08T21:33:44Z
updated_at: 2023-10-08T21:44:03Z
url: https://github.com/astral-sh/uv/pull/67
synced_at: 2026-01-12T16:03:43Z
```

# Use recursive `clonefile` calls on macOS

---

_@charliermarsh_

It turns out that on macOS, you can pass `clonefile` a directory to recursively copy an entire directory. This speeds up wheel installation dramatically, by about 3x.


---

_Merged by @charliermarsh on 2023-10-08 21:44_

---

_Closed by @charliermarsh on 2023-10-08 21:44_

---

_Branch deleted on 2023-10-08 21:44_

---
