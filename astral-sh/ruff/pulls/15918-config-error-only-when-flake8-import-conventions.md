```yaml
number: 15918
title: "Config error only when  `flake8-import-conventions`  alias conflicts with `isort.required-imports` bound name"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: alias-config
created_at: 2025-02-03T23:26:56Z
updated_at: 2025-02-04T23:05:36Z
url: https://github.com/astral-sh/ruff/pull/15918
synced_at: 2026-01-12T15:55:53Z
```

# Config error only when  `flake8-import-conventions`  alias conflicts with `isort.required-imports` bound name

---

_@dylwil3_

Previously an error was emitted any time the configuration required both an import of a module and an alias for that module. However, required imports could themselves contain an alias, which may or may not agree with the required alias.

To wit: requiring `import pandas as pd` does not conflict with the `flake8-import-conventions.alias` config `{"pandas":"pd"}`.

This PR refines the check before throwing an error.

Closes #15911 


---

_Label `bug` added by @dylwil3 on 2025-02-03 23:26_

---

_Label `configuration` added by @dylwil3 on 2025-02-03 23:26_

---

_@dylwil3 reviewed on 2025-02-03 23:27_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/configuration.rs`:1592 on 2025-02-03 23:27_

I'm open to replacing `qualified_name` with `bound_name` here if others think that error message makes more sense.

---

_Review requested from @ntBre by @dylwil3 on 2025-02-03 23:28_

---

_Comment by @github-actions[bot] on 2025-02-03 23:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_workspace/src/configuration.rs`:1592 on 2025-02-04 13:38_

I think `qualified_name` makes sense. That seems like the easiest way to find the error in the config to me, but I'm open to other opinions too. What does it look like if you print the whole `required_import`?

---

_@ntBre approved on 2025-02-04 13:38_

Looks great, thank you for fixing this!

---

_@dylwil3 reviewed on 2025-02-04 22:44_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/configuration.rs`:1592 on 2025-02-04 22:44_

I think the whole required import looks like a lot, so I'll keep it as `qualified_name`. 

---

_Merged by @dylwil3 on 2025-02-04 23:05_

---

_Closed by @dylwil3 on 2025-02-04 23:05_

---
