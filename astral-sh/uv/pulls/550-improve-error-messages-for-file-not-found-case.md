```yaml
number: 550
title: "Improve error messages for 'file not found' case"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/err
created_at: 2023-12-04T21:25:56Z
updated_at: 2023-12-04T22:01:52Z
url: https://github.com/astral-sh/uv/pull/550
synced_at: 2026-01-10T15:44:44Z
```

# Improve error messages for 'file not found' case

---

_Pull request opened by @charliermarsh on 2023-12-04 21:25_

Right now, if you specify a wheel that doesn't exist, you get: `no such file or directory` with no additional context. Oops!

---

_Label `bug` added by @charliermarsh on 2023-12-04 21:26_

---

_@charliermarsh reviewed on 2023-12-04 21:28_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:201 on 2023-12-04 21:28_

The real cause was not using `fs_err` here, but I polished things a bit further than that.

---

_@konstin approved on 2023-12-04 21:30_

I've seen this too, thanks!

---

_Merged by @charliermarsh on 2023-12-04 22:01_

---

_Closed by @charliermarsh on 2023-12-04 22:01_

---

_Branch deleted on 2023-12-04 22:01_

---
