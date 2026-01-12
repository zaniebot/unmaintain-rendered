```yaml
number: 822
title: Mark nonlocal variables as used in parent scopes
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/f841
created_at: 2022-11-20T00:19:22Z
updated_at: 2022-11-20T00:21:03Z
url: https://github.com/astral-sh/ruff/pull/822
synced_at: 2026-01-12T05:48:45Z
```

# Mark nonlocal variables as used in parent scopes

---

_Pull request opened by @charliermarsh on 2022-11-20 00:19_

In #800, I modified our handling of `StmtKind::Global` to avoid setting a binding in every parent scope. This inadvertently changed the behavior for `StmtKind::Nonlocal` too (I just missed that it was the same pattern branch), causing us to _not_ mark nonlocal variables as used at the defining site in the parent scope.

Resolves #820.


---

_Merged by @charliermarsh on 2022-11-20 00:21_

---

_Closed by @charliermarsh on 2022-11-20 00:21_

---

_Branch deleted on 2022-11-20 00:21_

---
