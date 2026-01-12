```yaml
number: 1731
title: Ignore invalid extras from PyPI
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-02-20T03:15:40Z
updated_at: 2024-02-20T04:40:46Z
url: https://github.com/astral-sh/uv/pull/1731
synced_at: 2026-01-12T16:04:42Z
```

# Ignore invalid extras from PyPI

---

_@charliermarsh_

## Summary

We don't control these, so it seems preferable _not_ to fail on them, but rather, to just ignore them entirely. (I considered adding a long allow-list, but then questioned the point of it? We'd end up having to extend it if more invalid extras were published in the future.)

Closes https://github.com/astral-sh/uv/issues/1633.


---

_Review requested from @zanieb by @charliermarsh on 2024-02-20 03:15_

---

_Label `compatibility` added by @charliermarsh on 2024-02-20 03:15_

---

_Marked ready for review by @charliermarsh on 2024-02-20 03:15_

---

_Merged by @charliermarsh on 2024-02-20 03:26_

---

_Closed by @charliermarsh on 2024-02-20 03:26_

---

_Branch deleted on 2024-02-20 03:26_

---

_Comment by @zanieb on 2024-02-20 04:40_

👍 makes sense to me

---
