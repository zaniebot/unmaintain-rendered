```yaml
number: 2423
title: Implement C414 autofix
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-31T22:44:06Z
updated_at: 2023-02-12T05:20:46Z
url: https://github.com/astral-sh/ruff/issues/2423
synced_at: 2026-01-12T15:54:42Z
```

# Implement C414 autofix

---

_@spaceone_

C414 unnecessary-double-cast-or-process should be fairly easy autofixable.

e.g. `sorted(list(foo))` → `sorted(foo)`

---

_Label `autofix` added by @charliermarsh on 2023-01-31 22:44_

---

_Comment by @sbrugman on 2023-01-31 22:46_

Will take this together with https://github.com/charliermarsh/ruff/issues/2262

---

_Closed by @charliermarsh on 2023-02-12 05:20_

---

_Closed by @charliermarsh on 2023-02-12 05:20_

---
