```yaml
number: 645
title: "Allow reporters to take `dyn Metadata`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/reporters
created_at: 2023-12-14T01:19:39Z
updated_at: 2023-12-14T12:51:41Z
url: https://github.com/astral-sh/uv/pull/645
synced_at: 2026-01-12T16:04:05Z
```

# Allow reporters to take `dyn Metadata`

---

_@charliermarsh_

_No description provided._

---

_@konstin approved on 2023-12-14 11:36_

Thanks!

---

_Merged by @konstin on 2023-12-14 11:36_

---

_Closed by @konstin on 2023-12-14 11:36_

---

_Branch deleted on 2023-12-14 11:36_

---

_Comment by @konstin on 2023-12-14 12:51_

Oh no, this doesn't actually fix the problem :/ `LocalEditable` can't implement `Metadata` because we don't have a package name.

---
