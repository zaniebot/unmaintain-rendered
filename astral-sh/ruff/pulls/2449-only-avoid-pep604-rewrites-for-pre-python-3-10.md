```yaml
number: 2449
title: Only avoid PEP604 rewrites for pre-Python 3.10 code
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/glob
created_at: 2023-02-01T17:30:34Z
updated_at: 2023-02-01T18:03:52Z
url: https://github.com/astral-sh/ruff/pull/2449
synced_at: 2026-01-12T04:52:00Z
```

# Only avoid PEP604 rewrites for pre-Python 3.10 code

---

_Pull request opened by @charliermarsh on 2023-02-01 17:30_

I moved the `self.in_annotation` guard out of the version check in #1563. But, I think that was a mistake. It was done to resolve #1560, but the fix in that case _should've_ been to set a different Python version.

Closes #2447.

---

_Label `bug` added by @charliermarsh on 2023-02-01 17:56_

---

_Merged by @charliermarsh on 2023-02-01 18:03_

---

_Closed by @charliermarsh on 2023-02-01 18:03_

---

_Branch deleted on 2023-02-01 18:03_

---
