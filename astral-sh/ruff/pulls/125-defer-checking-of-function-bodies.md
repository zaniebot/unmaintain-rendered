```yaml
number: 125
title: Defer checking of function bodies
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/lifetime
created_at: 2022-09-08T02:14:41Z
updated_at: 2022-09-08T02:34:44Z
url: https://github.com/astral-sh/ruff/pull/125
synced_at: 2026-01-12T05:48:45Z
```

# Defer checking of function bodies

---

_Pull request opened by @charliermarsh on 2022-09-08 02:14_

This PR resolves #119 by deferring the traversal of function bodies until after traversing the rest of the module. This required introducing a lifetime parameter to `Visitor`, and tracking a bunch of additional state on the `Checker`. The borrow checker did not like this change, took many iterations...


---

_Merged by @charliermarsh on 2022-09-08 02:34_

---

_Closed by @charliermarsh on 2022-09-08 02:34_

---

_Branch deleted on 2022-09-08 02:34_

---
