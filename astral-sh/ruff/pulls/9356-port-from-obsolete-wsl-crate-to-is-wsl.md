```yaml
number: 9356
title: Port from obsolete wsl crate to is-wsl
type: pull_request
state: merged
author: decathorpe
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2024-01-02T12:57:34Z
updated_at: 2024-01-02T13:15:53Z
url: https://github.com/astral-sh/ruff/pull/9356
synced_at: 2026-01-10T23:07:18Z
```

# Port from obsolete wsl crate to is-wsl

---

_Pull request opened by @decathorpe on 2024-01-02 12:57_

The "wsl" crate was last touched in 2019, whereas the "is-wsl" crate was last updated in 2023. Additionally, it is unclear whether the "wsl" crate supports both WSL1 and WSL2 (which was announced in 2019), whereas the "is-wsl" crate explicitly supports both WSL1 and WSL2.

The required code changes are minimal, since both crates provide only a `is_wsl() -> bool` function.

---

_Comment by @github-actions[bot] on 2024-01-02 13:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-01-02 13:14_

Thanks!

---

_Merged by @charliermarsh on 2024-01-02 13:14_

---

_Closed by @charliermarsh on 2024-01-02 13:14_

---

_Comment by @decathorpe on 2024-01-02 13:15_

Wow, that was *super fast* 😱  Thank you!

---
