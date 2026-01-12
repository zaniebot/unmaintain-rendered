```yaml
number: 953
title: Rewrite type annotations on Python 3.7 when __future__ enabled
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/deferred
created_at: 2022-11-29T01:55:57Z
updated_at: 2022-11-29T01:57:39Z
url: https://github.com/astral-sh/ruff/pull/953
synced_at: 2026-01-12T05:48:46Z
```

# Rewrite type annotations on Python 3.7 when __future__ enabled

---

_Pull request opened by @charliermarsh on 2022-11-29 01:55_

Resolves #949.

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1200 on 2022-11-29 01:57_

`pyupgrade` seems to allow this on any Python version (if an annotation, with `__future__` enabled). I can't find a reference, so just keeping to 3.7 and above for now.

---

_@charliermarsh reviewed on 2022-11-29 01:57_

---

_Merged by @charliermarsh on 2022-11-29 01:57_

---

_Closed by @charliermarsh on 2022-11-29 01:57_

---

_Branch deleted on 2022-11-29 01:57_

---
