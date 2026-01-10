```yaml
number: 601
title: "Use async `Command` for wheel build operations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/command
created_at: 2023-12-09T16:09:34Z
updated_at: 2023-12-11T08:40:32Z
url: https://github.com/astral-sh/uv/pull/601
synced_at: 2026-01-10T15:44:44Z
```

# Use async `Command` for wheel build operations

---

_Pull request opened by @charliermarsh on 2023-12-09 16:09_

Incredibly, this speeds up the install on a large project from 2m6s to 50s.


---

_Label `performance` added by @charliermarsh on 2023-12-09 16:10_

---

_Merged by @charliermarsh on 2023-12-09 16:20_

---

_Closed by @charliermarsh on 2023-12-09 16:20_

---

_Branch deleted on 2023-12-09 16:20_

---

_Comment by @konstin on 2023-12-11 08:40_

So we didn't actually build in parallel - thanks for figuring out this bottleneck!

---
