```yaml
number: 3362
title: "Don't enforce typing-import rules in .pyi files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tch
created_at: 2023-03-06T16:37:16Z
updated_at: 2023-03-06T20:03:37Z
url: https://github.com/astral-sh/ruff/pull/3362
synced_at: 2026-01-12T04:39:44Z
```

# Don't enforce typing-import rules in .pyi files

---

_Pull request opened by @charliermarsh on 2023-03-06 16:37_

`.pyi` files aren't executed at runtime, so moving imports into (or even out of) `TYPE_CHECKING` blocks is just noisy feedback.

Closes #3357.

---

_Review requested from @konstin by @charliermarsh on 2023-03-06 16:37_

---

_Label `bug` added by @charliermarsh on 2023-03-06 16:37_

---

_Merged by @charliermarsh on 2023-03-06 20:03_

---

_Closed by @charliermarsh on 2023-03-06 20:03_

---

_Branch deleted on 2023-03-06 20:03_

---
