```yaml
number: 2946
title: Fix add-required-import with multi-line offsets
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/required-import
created_at: 2023-02-16T03:22:12Z
updated_at: 2023-02-16T03:24:57Z
url: https://github.com/astral-sh/ruff/pull/2946
synced_at: 2026-01-12T15:55:12Z
```

# Fix add-required-import with multi-line offsets

---

_@charliermarsh_

Closes #2944.

---

_Label `bug` added by @charliermarsh on 2023-02-16 03:22_

---

_@charliermarsh reviewed on 2023-02-16 03:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/helpers.rs`:113 on 2023-02-16 03:22_

We're generating an offset here, so it needs to be relative to the current `splice`!

---

_Merged by @charliermarsh on 2023-02-16 03:24_

---

_Closed by @charliermarsh on 2023-02-16 03:24_

---

_Branch deleted on 2023-02-16 03:24_

---
