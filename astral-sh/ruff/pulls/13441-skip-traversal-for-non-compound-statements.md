```yaml
number: 13441
title: Skip traversal for non-compound statements
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/traverse
created_at: 2024-09-21T18:54:34Z
updated_at: 2024-09-26T09:36:21Z
url: https://github.com/astral-sh/ruff/pull/13441
synced_at: 2026-01-12T15:55:44Z
```

# Skip traversal for non-compound statements

---

_@charliermarsh_

## Summary

None of these can contain imports.


---

_Label `performance` added by @charliermarsh on 2024-09-21 18:54_

---

_Comment by @github-actions[bot] on 2024-09-21 19:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_graph/src/collector.rs`:47 on 2024-09-21 20:00_

Can we also skip over all expressions?


---

_@MichaReiser reviewed on 2024-09-21 20:01_

---

_@charliermarsh reviewed on 2024-09-21 20:10_

---

_Review comment by @charliermarsh on `crates/ruff_graph/src/collector.rs`:47 on 2024-09-21 20:10_

Yeah this already does, right? We're skipping over everything except compound statements.

---

_Merged by @charliermarsh on 2024-09-21 20:47_

---

_Closed by @charliermarsh on 2024-09-21 20:47_

---

_Branch deleted on 2024-09-21 20:47_

---

_@MichaReiser reviewed on 2024-09-26 09:36_

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/collector.rs`:49 on 2024-09-26 09:36_

Won't this skip over `StmtImportFrom` imports if `self.string_imports` is `false`?

---
