```yaml
number: 1260
title: Use anstream consistently and remove clippy lints
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/anstream-everywhere
created_at: 2024-02-06T19:27:59Z
updated_at: 2024-02-06T21:16:27Z
url: https://github.com/astral-sh/uv/pull/1260
synced_at: 2026-01-10T15:33:24Z
```

# Use anstream consistently and remove clippy lints

---

_Pull request opened by @konstin on 2024-02-06 19:27_

We need to use the anstream print macros instead of the std print macros, otherwise we risk wrong color behavior (https://github.com/astral-sh/puffin/pull/1258#discussion_r1480428236). Luckily, the `print_stderr` and `print_stdout` lints catch usages of the std prints.

This PR switches over to anstream consistently and removes the now redundant clippy lints. The lints should catch missing anstream usage in the future.

---

_Label `bug` added by @konstin on 2024-02-06 19:27_

---

_Review requested from @charliermarsh by @konstin on 2024-02-06 19:27_

---

_Review requested from @zanieb by @konstin on 2024-02-06 19:27_

---

_@charliermarsh approved on 2024-02-06 19:29_

---

_Comment by @charliermarsh on 2024-02-06 19:29_

We do now run the risk of accidentally shipping a print... But that was already true.

---

_@zanieb approved on 2024-02-06 20:39_

---

_Merged by @konstin on 2024-02-06 21:16_

---

_Closed by @konstin on 2024-02-06 21:16_

---

_Branch deleted on 2024-02-06 21:16_

---
