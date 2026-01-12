```yaml
number: 168
title: Update worker to avoid downloading entire wheel
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/worker
created_at: 2023-10-23T01:05:14Z
updated_at: 2023-10-23T06:47:54Z
url: https://github.com/astral-sh/uv/pull/168
synced_at: 2026-01-12T16:03:47Z
```

# Update worker to avoid downloading entire wheel

---

_@charliermarsh_

The previous solution downloaded the entire wheel in-memory. This solution reads lazily.

Closes https://github.com/astral-sh/puffin/issues/153.


---

_Merged by @charliermarsh on 2023-10-23 01:07_

---

_Closed by @charliermarsh on 2023-10-23 01:07_

---

_Branch deleted on 2023-10-23 01:07_

---

_Comment by @konstin on 2023-10-23 06:47_

neat! 

---
