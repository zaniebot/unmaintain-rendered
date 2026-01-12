```yaml
number: 2420
title: Avoid implicit-namespace-package checks for .pyi files
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/imp
created_at: 2023-01-31T21:46:56Z
updated_at: 2023-01-31T22:35:32Z
url: https://github.com/astral-sh/ruff/pull/2420
synced_at: 2026-01-12T15:55:08Z
```

# Avoid implicit-namespace-package checks for .pyi files

---

_@charliermarsh_

Closes #2328.

---

_Label `bug` added by @charliermarsh on 2023-01-31 21:46_

---

_@spaceone reviewed on 2023-01-31 21:49_

---

_Review comment by @spaceone on `src/rules/flake8_no_pep420/rules.rs`:23 on 2023-01-31 21:49_

```suggestion
    if package.is_none() && ! path.extension().map_or(false, |ext| ext == "pyi") {
```
otherwise I guess all files without file extensions are ignored?

---

_@charliermarsh reviewed on 2023-01-31 21:55_

---

_Review comment by @charliermarsh on `src/rules/flake8_no_pep420/rules.rs`:23 on 2023-01-31 21:55_

Hmm -- fair!

---

_Merged by @charliermarsh on 2023-01-31 22:35_

---

_Closed by @charliermarsh on 2023-01-31 22:35_

---

_Branch deleted on 2023-01-31 22:35_

---
