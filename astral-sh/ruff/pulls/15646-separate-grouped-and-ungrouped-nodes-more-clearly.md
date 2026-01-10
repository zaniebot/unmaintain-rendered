```yaml
number: 15646
title: Separate grouped and ungrouped nodes more clearly in AST generator
type: pull_request
state: merged
author: dcreager
labels:
  - internal
assignees: []
merged: true
base: main
head: dcreager/grouped-nodes
created_at: 2025-01-21T15:38:44Z
updated_at: 2025-01-21T18:37:20Z
url: https://github.com/astral-sh/ruff/pull/15646
synced_at: 2026-01-10T20:05:43Z
```

# Separate grouped and ungrouped nodes more clearly in AST generator

---

_Pull request opened by @dcreager on 2025-01-21 15:38_

This is a minor cleanup to the AST generation script to make a clearer separation between nodes that do appear in a group enum, and those that don't.  There are some types and methods that we create for every syntax node, and others that refer to the group that the syntax node belongs to, and which therefore don't make sense for ungrouped nodes. This new separation makes it clearer which category each definition is in, since you're either inside of a `for group in ast.groups` loop, or a `for node in ast.all_nodes` loop.



---

_Review comment by @dcreager on `crates/ruff_python_ast/src/generated.rs`:1 on 2025-01-21 15:42_

There are changes in this file because a few definitions moved around, but their content did not change

---

_@dcreager reviewed on 2025-01-21 15:42_

---

_Comment by @github-actions[bot] on 2025-01-21 15:47_

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

_Label `internal` added by @MichaReiser on 2025-01-21 17:24_

---

_@MichaReiser approved on 2025-01-21 17:25_

---

_Merged by @dcreager on 2025-01-21 18:37_

---

_Closed by @dcreager on 2025-01-21 18:37_

---

_Branch deleted on 2025-01-21 18:37_

---
