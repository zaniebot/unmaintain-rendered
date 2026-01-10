```yaml
number: 13409
title: "[red-knot] visit with-item vars even if not a Name"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/with-non-name
created_at: 2024-09-19T16:55:21Z
updated_at: 2024-09-19T17:37:51Z
url: https://github.com/astral-sh/ruff/pull/13409
synced_at: 2026-01-10T21:30:32Z
```

# [red-knot] visit with-item vars even if not a Name

---

_Pull request opened by @carljm on 2024-09-19 16:55_

This fixes the last panic on checking pandas.

(Match statement became an `if let` because clippy decided it wanted that once I added the additional line in the else case?)


---

_Label `red-knot` added by @carljm on 2024-09-19 16:55_

---

_Review requested from @MichaReiser by @carljm on 2024-09-19 16:55_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-19 16:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/resources/test/corpus/67_with_non_name_target.py`:1 on 2024-09-19 16:59_

I think we should see if we can setup a test that runs on all the test Python snippets we have for the parser as well. We have very extensive parser coverage for `with` statements; it would have caught this :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:935 on 2024-09-19 17:00_

```suggestion
            let target = item.optional_vars.as_deref();
            if let Some(ast::Expr::Name(name)) = target {
                self.infer_definition(name);
            } else {
                // TODO infer definitions in unpacking assignment
                self.infer_expression(&item.context_expr);
                self.infer_optional_expression(target);
            }
```

---

_@AlexWaygood approved on 2024-09-19 17:00_

---

_Comment by @github-actions[bot] on 2024-09-19 17:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2024-09-19 17:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:935 on 2024-09-19 17:10_

I thought about this and decided not to bother 😆 I would be very surprised if it makes any difference to the generated code. But sure, doesn't hurt, it's a bit nicer to read.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:935 on 2024-09-19 17:12_

Oh totally, I just thought it made the code less repetitive and a tad more readable!

---

_@AlexWaygood reviewed on 2024-09-19 17:12_

---

_Merged by @carljm on 2024-09-19 17:37_

---

_Closed by @carljm on 2024-09-19 17:37_

---

_Branch deleted on 2024-09-19 17:37_

---
