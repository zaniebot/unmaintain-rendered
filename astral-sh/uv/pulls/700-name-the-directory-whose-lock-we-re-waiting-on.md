```yaml
number: 700
title: "Name the directory whose lock we're waiting on"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/which-lock
created_at: 2023-12-19T12:14:58Z
updated_at: 2023-12-19T13:39:25Z
url: https://github.com/astral-sh/uv/pull/700
synced_at: 2026-01-10T15:44:44Z
```

# Name the directory whose lock we're waiting on

---

_Pull request opened by @konstin on 2023-12-19 12:14_

_No description provided._

---

_Merged by @konstin on 2023-12-19 12:19_

---

_Closed by @konstin on 2023-12-19 12:19_

---

_Branch deleted on 2023-12-19 12:19_

---

_@charliermarsh reviewed on 2023-12-19 13:39_

---

_Review comment by @charliermarsh on `crates/puffin-fs/src/lib.rs`:109 on 2023-12-19 13:39_

I think this is going to be incorrect for the Git locks case, where we aren’t locking a directory. We need a separate abstraction or a way to pass in the thing being locked as a separate argument.

---
