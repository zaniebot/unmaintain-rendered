```yaml
number: 8950
title: "ruff_python_formatter: light refactoring of code snippet formatting in docstrings"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - docstring
  - formatter
assignees: []
merged: true
base: main
head: ag/fmt/refactor-for-rest
created_at: 2023-12-01T16:39:45Z
updated_at: 2023-12-01T19:46:40Z
url: https://github.com/astral-sh/ruff/pull/8950
synced_at: 2026-01-10T23:40:55Z
```

# ruff_python_formatter: light refactoring of code snippet formatting in docstrings

---

_Pull request opened by @BurntSushi on 2023-12-01 16:39_

In the source of working on #8859, I made a number of smallish refactors to how code snippet formatting works. Most or all of these were motivated by writing in support for reStructuredText blocks. They have some fundamentally different requirements than doctests, and there are a lot more ways for reStructuredText blocks to become invalid.

(Commit-by-commit review is recommended as the commit messages provide further context on each change. I split this off from ongoing work to make review more manageable.)

---

_Review requested from @MichaReiser by @BurntSushi on 2023-12-01 16:39_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-01 16:39_

---

_Label `internal` added by @BurntSushi on 2023-12-01 16:41_

---

_Label `docstring` added by @BurntSushi on 2023-12-01 16:41_

---

_Label `formatter` added by @BurntSushi on 2023-12-01 16:41_

---

_Comment by @github-actions[bot] on 2023-12-01 16:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2023-12-01 18:21_

---

_Merged by @BurntSushi on 2023-12-01 19:46_

---

_Closed by @BurntSushi on 2023-12-01 19:46_

---

_Branch deleted on 2023-12-01 19:46_

---
