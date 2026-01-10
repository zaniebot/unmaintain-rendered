```yaml
number: 6338
title: "Don't drop download error source"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/dont-drop-error-source
created_at: 2024-08-21T15:07:57Z
updated_at: 2024-08-21T18:15:58Z
url: https://github.com/astral-sh/uv/pull/6338
synced_at: 2026-01-10T13:09:51Z
```

# Don't drop download error source

---

_Pull request opened by @konstin on 2024-08-21 15:07_

Part of #6331


---

_Review requested from @charliermarsh by @konstin on 2024-08-21 15:07_

---

_Label `error messages` added by @konstin on 2024-08-21 15:08_

---

_@zanieb approved on 2024-08-21 15:10_

Should we be using a miette report here?

---

_Comment by @konstin on 2024-08-21 15:17_

As we're it using it elsewhere in uv, miette always discards the error sources, so we couldn't use it here the same way.

---

_Merged by @zanieb on 2024-08-21 18:15_

---

_Closed by @zanieb on 2024-08-21 18:15_

---

_Branch deleted on 2024-08-21 18:15_

---
