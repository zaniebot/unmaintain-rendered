```yaml
number: 441
title: Avoid using incompatible wheels for source distribution-less packages
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/any-wheel
created_at: 2023-11-17T02:03:56Z
updated_at: 2023-11-17T02:10:55Z
url: https://github.com/astral-sh/uv/pull/441
synced_at: 2026-01-12T16:03:57Z
```

# Avoid using incompatible wheels for source distribution-less packages

---

_@charliermarsh_

We're willing to use platform-incompatible wheels during resolution, to quicken access to metadata... But we should avoid choosing an incompatible wheel if the package lacks a source distribution since, in that case, we definitely won't be able to install it.

Closes https://github.com/astral-sh/puffin/issues/439.

---

_Marked ready for review by @charliermarsh on 2023-11-17 02:04_

---

_Label `bug` added by @charliermarsh on 2023-11-17 02:04_

---

_Merged by @charliermarsh on 2023-11-17 02:10_

---

_Closed by @charliermarsh on 2023-11-17 02:10_

---

_Branch deleted on 2023-11-17 02:10_

---
