```yaml
number: 16136
title: "TEST: Make `symbol_by_id` not a salsa query"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - do-not-merge
  - ty
assignees: []
draft: true
base: main
head: dhruv/symbol-not-a-query
created_at: 2025-02-13T10:03:47Z
updated_at: 2025-02-14T09:56:30Z
url: https://github.com/astral-sh/ruff/pull/16136
synced_at: 2026-01-12T15:55:53Z
```

# TEST: Make `symbol_by_id` not a salsa query

---

_@dhruvmanila_

Just testing this out, I did see a 4% regression on the incremental benchmark locally but wanted to get a more accurate number.

---

_Label `do-not-merge` added by @dhruvmanila on 2025-02-13 10:03_

---

_@MichaReiser reviewed on 2025-02-13 10:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:124 on 2025-02-13 10:08_

We'll need a salsa query somewhere because evaluating the visibility constraint access `.node_ref` but we can consider moving the salsa query closer to where it is needed?

---

_Comment by @github-actions[bot] on 2025-02-13 10:13_

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

_Label `red-knot` added by @AlexWaygood on 2025-02-13 18:46_

---

_Closed by @dhruvmanila on 2025-02-14 09:56_

---
