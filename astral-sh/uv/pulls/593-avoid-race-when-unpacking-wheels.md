```yaml
number: 593
title: Avoid race when unpacking wheels
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/collide
created_at: 2023-12-08T17:40:27Z
updated_at: 2023-12-08T17:46:20Z
url: https://github.com/astral-sh/uv/pull/593
synced_at: 2026-01-12T16:04:03Z
```

# Avoid race when unpacking wheels

---

_@charliermarsh_

## Summary

If someone else beats us to the unzip, we should let them win.

We already have a check for this at the top of the unzip method, but it's also possible that two source distributions get built in parallel that both try to unpack the same build dependency.

---

_Merged by @charliermarsh on 2023-12-08 17:46_

---

_Closed by @charliermarsh on 2023-12-08 17:46_

---

_Branch deleted on 2023-12-08 17:46_

---
