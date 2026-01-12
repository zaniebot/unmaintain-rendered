```yaml
number: 691
title: "Default to isort's import sort logic"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/isort-order
created_at: 2022-11-12T03:41:19Z
updated_at: 2022-11-12T03:41:40Z
url: https://github.com/astral-sh/ruff/pull/691
synced_at: 2026-01-12T15:55:05Z
```

# Default to isort's import sort logic

---

_@charliermarsh_

We now resolve to isort's sorting logic -- which is a little complicated, but in short:

- Modules are sorted in a case-insensitive way.
- Module _members_ (the targets in an `import from`) are sorted based on a heuristic that tries to order constants, then classes, then all other variables.

Resolves #673.


---

_Merged by @charliermarsh on 2022-11-12 03:41_

---

_Closed by @charliermarsh on 2022-11-12 03:41_

---

_Branch deleted on 2022-11-12 03:41_

---
