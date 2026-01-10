```yaml
number: 12896
title: "[flake8-async] Do not lint yield in context manager `cancel-scope-no-checkpoint (ASYNC100)`"
type: pull_request
state: merged
author: dylwil3
labels: []
assignees: []
merged: true
base: main
head: async100-skip-yield-in-context-manager
created_at: 2024-08-14T19:21:05Z
updated_at: 2024-08-15T02:27:00Z
url: https://github.com/astral-sh/ruff/pull/12896
synced_at: 2026-01-10T21:38:32Z
```

# [flake8-async] Do not lint yield in context manager `cancel-scope-no-checkpoint (ASYNC100)`

---

_Pull request opened by @dylwil3 on 2024-08-14 19:21_

For compatibility with upstream, treat `yield` as a checkpoint inside cancel scopes.

Closes #12873.


---

_Comment by @github-actions[bot] on 2024-08-14 19:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_async/rules/cancel_scope_no_checkpoint.rs`:90 on 2024-08-14 19:39_

The logic here only traverses the direct children of a with statement but, for example, not the statements in an if body. Is this intentional? If not, consider using the StatementVisitor

---

_@MichaReiser reviewed on 2024-08-14 19:39_

---

_@dylwil3 reviewed on 2024-08-14 20:25_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_async/rules/cancel_scope_no_checkpoint.rs`:90 on 2024-08-14 20:25_

Not intentional, thank you! I tried using `any_over_body` instead. Unless I'm misunderstanding, that's slightly faster since it gets to stop traversing the tree as soon as a `yield` is found, right?

---

_@MichaReiser reviewed on 2024-08-14 20:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_async/rules/cancel_scope_no_checkpoint.rs`:90 on 2024-08-14 20:37_

I'm not familiar with that method. I'll take a look tomorrow 

---

_@charliermarsh reviewed on 2024-08-15 00:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_async/rules/cancel_scope_no_checkpoint.rs`:90 on 2024-08-15 00:58_

Yup, correct usage of `any_over_body`!

---

_@charliermarsh approved on 2024-08-15 00:58_

---

_Merged by @charliermarsh on 2024-08-15 01:02_

---

_Closed by @charliermarsh on 2024-08-15 01:02_

---

_Branch deleted on 2024-08-15 02:27_

---
