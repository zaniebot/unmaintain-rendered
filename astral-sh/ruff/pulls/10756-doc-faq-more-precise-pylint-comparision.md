```yaml
number: 10756
title: "doc(FAQ): More precise PyLint comparision"
type: pull_request
state: merged
author: buhtz
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-04-03T12:00:55Z
updated_at: 2024-04-05T22:12:33Z
url: https://github.com/astral-sh/ruff/pull/10756
synced_at: 2026-01-12T15:55:33Z
```

# doc(FAQ): More precise PyLint comparision

---

_@buhtz_

Section about comparing Ruff to PyLint now is more precise about the following two points:
 - Ruff do count branches different and there for earlier give too-many-branches warning. 
 - Activating all Pylint rules in Ruff also activates pylint rules that are not active by default in Pylint itself because they are implemented via pylint plugins.

---

_@charliermarsh approved on 2024-04-05 22:10_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-04-05 22:10_

---

_Merged by @charliermarsh on 2024-04-05 22:12_

---

_Closed by @charliermarsh on 2024-04-05 22:12_

---
