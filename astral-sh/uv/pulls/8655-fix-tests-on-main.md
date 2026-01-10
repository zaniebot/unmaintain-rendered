```yaml
number: 8655
title: Fix tests on main
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/fix-main-contraint-less-3
created_at: 2024-10-29T10:37:42Z
updated_at: 2024-10-29T13:53:08Z
url: https://github.com/astral-sh/uv/pull/8655
synced_at: 2026-01-10T12:54:14Z
```

# Fix tests on main

---

_Pull request opened by @konstin on 2024-10-29 10:37_

markupsafe 3 broke the tests on main.

Crude fix but it makes main pass again.

---

_Review requested from @charliermarsh by @konstin on 2024-10-29 10:37_

---

_Merged by @konstin on 2024-10-29 10:46_

---

_Closed by @konstin on 2024-10-29 10:46_

---

_Branch deleted on 2024-10-29 10:46_

---

_Comment by @zanieb on 2024-10-29 13:52_

Do these tests not use `exclude-newer`?

---

_Comment by @charliermarsh on 2024-10-29 13:53_

No, they use the PyTorch index.

---
