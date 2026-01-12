```yaml
number: 2474
title: Move flake8-bugbear violations to rule modules
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: move-bugbear-violations
created_at: 2023-02-02T13:11:49Z
updated_at: 2023-02-02T14:34:27Z
url: https://github.com/astral-sh/ruff/pull/2474
synced_at: 2026-01-12T04:52:00Z
```

# Move flake8-bugbear violations to rule modules

---

_Pull request opened by @akx on 2023-02-02 13:11_

As is the modern way!

---

_Review comment by @charliermarsh on `src/rules/flake8_bugbear/violations.rs`:14 on 2023-02-02 13:19_

I think the even more desired goal is to move these into the files in which the rule logic is defined. That way we can have the violation definition, the logic, and all its documentation in one module.

---

_@charliermarsh reviewed on 2023-02-02 13:19_

---

_Converted to draft by @akx on 2023-02-02 13:20_

---

_Marked ready for review by @akx on 2023-02-02 14:19_

---

_Renamed from "Move flake8-bugbear violations to separate module" to "Move flake8-bugbear violations to rule modules" by @akx on 2023-02-02 14:19_

---

_Merged by @charliermarsh on 2023-02-02 14:34_

---

_Closed by @charliermarsh on 2023-02-02 14:34_

---
