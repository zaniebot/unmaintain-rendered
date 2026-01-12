```yaml
number: 4349
title: "Add a specialized `StatementVisitor`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/stmt-visitor
created_at: 2023-05-10T15:26:52Z
updated_at: 2023-05-10T16:42:21Z
url: https://github.com/astral-sh/ruff/pull/4349
synced_at: 2026-01-12T03:56:39Z
```

# Add a specialized `StatementVisitor`

---

_Pull request opened by @charliermarsh on 2023-05-10 15:26_

## Summary

This PR adds a specialized `StatementVisitor` trait that structs can implement in lieu of the generic `Visitor` when they only care about visiting statements (recursively). In many cases, we only care about visiting statements, and visiting the entire AST is thus quite wasteful, as expressions can't contain statements.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-10 15:26_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/yield_in_for_loop.rs`:113 on 2023-05-10 15:30_

Just to confirm: It's intentional that the visitor now no longer recurses for expressions that are not explicitly listed in the deleted match statement?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/tryceratops/rules/type_check_without_type_error.rs`:72 on 2023-05-10 15:30_

Is it an intentional change that the visitor now no-longer recurses for any not-`ListComp`, `DictComp`, `SetComp`, or `Generator` expression?

---

_@MichaReiser approved on 2023-05-10 15:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/yield_in_for_loop.rs`:113 on 2023-05-10 15:33_

Yeah. I think this must've just been an oversight on my part, that those expressions couldn't contain statements anyway.

---

_@charliermarsh reviewed on 2023-05-10 15:33_

---

_Comment by @github-actions[bot] on 2023-05-10 16:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-10 16:42_

---

_Closed by @charliermarsh on 2023-05-10 16:42_

---

_Branch deleted on 2023-05-10 16:42_

---
