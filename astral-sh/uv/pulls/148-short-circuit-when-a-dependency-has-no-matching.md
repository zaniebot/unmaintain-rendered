```yaml
number: 148
title: Short-circuit when a dependency has no matching versions
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/short
created_at: 2023-10-20T03:47:12Z
updated_at: 2023-10-20T03:49:21Z
url: https://github.com/astral-sh/uv/pull/148
synced_at: 2026-01-12T16:03:46Z
```

# Short-circuit when a dependency has no matching versions

---

_@charliermarsh_

Kind of an oversight in my initial implementation. If we find that any package has _no_ matching versions, we should select it! This lets us short-circuit _immediately_ when top-level dependencies aren't satisfiable.

---

_Merged by @charliermarsh on 2023-10-20 03:49_

---

_Closed by @charliermarsh on 2023-10-20 03:49_

---

_Branch deleted on 2023-10-20 03:49_

---
