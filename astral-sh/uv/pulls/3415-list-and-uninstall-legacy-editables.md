```yaml
number: 3415
title: List and uninstall legacy editables
type: pull_request
state: merged
author: hauntsaninja
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: sj/egg2
created_at: 2024-05-06T21:46:12Z
updated_at: 2024-05-07T05:11:10Z
url: https://github.com/astral-sh/uv/pull/3415
synced_at: 2026-01-10T14:37:54Z
```

# List and uninstall legacy editables

---

_Pull request opened by @hauntsaninja on 2024-05-06 21:46_

Reopening #3396

---

_Comment by @charliermarsh on 2024-05-06 23:01_

Thanks, this is looking good. I'll try and merge it tonight.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-05-06 23:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-06 23:01_

---

_Label `enhancement` added by @charliermarsh on 2024-05-06 23:01_

---

_Label `compatibility` added by @charliermarsh on 2024-05-06 23:01_

---

_@charliermarsh reviewed on 2024-05-07 03:39_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/uninstall.rs`:261 on 2024-05-07 03:39_

I think we need to do a case-insensitive compare here on Windows, maybe?

https://github.com/pypa/pip/blob/41587f5e0017bcd849f42b314dc8a34a7db75621/src/pip/_internal/req/req_uninstall.py#L600

---

_@charliermarsh approved on 2024-05-07 03:43_

---

_Merged by @charliermarsh on 2024-05-07 03:51_

---

_Closed by @charliermarsh on 2024-05-07 03:51_

---

_Branch deleted on 2024-05-07 05:11_

---
