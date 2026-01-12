```yaml
number: 3236
title: "[flake8-django] DJ003, DJ006, DJ007"
type: pull_request
state: merged
author: lkh42t
labels:
  - rule
assignees: []
merged: true
base: main
head: flake8-django
created_at: 2023-02-26T10:08:46Z
updated_at: 2023-03-22T12:06:46Z
url: https://github.com/astral-sh/ruff/pull/3236
synced_at: 2026-01-12T04:39:44Z
```

# [flake8-django] DJ003, DJ006, DJ007

---

_Pull request opened by @lkh42t on 2023-02-26 10:08_

Implements [flake8-django](https://github.com/rocioar/flake8-django) rules:
- DJ03
- DJ06
- DJ07

---

_Label `rule` added by @charliermarsh on 2023-02-26 22:26_

---

_Comment by @charliermarsh on 2023-02-26 22:27_

Awesome, thank you!

---

_@charliermarsh reviewed on 2023-02-26 22:27_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:825 on 2023-02-26 22:27_

I split this into two methods. I know it's not as DRY, and a little less efficient, but I think it's easier to think about them as independent rules.

---

_Merged by @charliermarsh on 2023-02-26 22:29_

---

_Closed by @charliermarsh on 2023-02-26 22:29_

---

_Branch deleted on 2023-03-22 12:06_

---
