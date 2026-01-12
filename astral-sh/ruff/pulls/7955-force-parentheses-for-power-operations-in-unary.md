```yaml
number: 7955
title: Force parentheses for power operations in unary expressions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/bin-op
created_at: 2023-10-13T20:48:40Z
updated_at: 2023-10-16T01:41:52Z
url: https://github.com/astral-sh/ruff/pull/7955
synced_at: 2026-01-12T15:55:25Z
```

# Force parentheses for power operations in unary expressions

---

_@charliermarsh_

## Summary

E.g., given `-10**100`, reformat as `-(10**100)`.

Black special cases this (https://github.com/psf/black/pull/909) and it's currently a deviation.

Closes https://github.com/astral-sh/ruff/issues/7951.


---

_Review requested from @konstin by @charliermarsh on 2023-10-13 20:48_

---

_Label `bug` added by @charliermarsh on 2023-10-13 20:48_

---

_Label `formatter` added by @charliermarsh on 2023-10-13 20:48_

---

_@MichaReiser approved on 2023-10-16 01:29_

---

_Merged by @charliermarsh on 2023-10-16 01:41_

---

_Closed by @charliermarsh on 2023-10-16 01:41_

---

_Branch deleted on 2023-10-16 01:41_

---
