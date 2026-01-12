```yaml
number: 719
title: Implement B021
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: B021
created_at: 2022-11-13T14:42:16Z
updated_at: 2022-11-13T16:40:24Z
url: https://github.com/astral-sh/ruff/pull/719
synced_at: 2026-01-12T15:55:05Z
```

# Implement B021

---

_@harupy_

#389

---

_Review comment by @charliermarsh on `src/check_ast.rs`:457 on 2022-11-13 15:57_

I think we also need to add this to `fn visit_docstring`, which checks the module-level docstring.

---

_@charliermarsh reviewed on 2022-11-13 15:57_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:346 on 2022-11-13 15:58_

Could we move this (and the class version) to just before `let definition = docstrings::extraction::extract(...)`? That way it'd at least be colocated with the other docstring-tracking code.

---

_@charliermarsh reviewed on 2022-11-13 15:58_

---

_Merged by @charliermarsh on 2022-11-13 16:40_

---

_Closed by @charliermarsh on 2022-11-13 16:40_

---
