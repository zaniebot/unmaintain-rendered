```yaml
number: 13405
title: "[refurb] Skip `slice-to-remove-prefix-or-suffix (FURB188)` when nontrivial slice step is present"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: slice-step
created_at: 2024-09-19T04:15:22Z
updated_at: 2024-09-19T16:48:02Z
url: https://github.com/astral-sh/ruff/pull/13405
synced_at: 2026-01-10T21:30:32Z
```

# [refurb] Skip `slice-to-remove-prefix-or-suffix (FURB188)` when nontrivial slice step is present

---

_Pull request opened by @dylwil3 on 2024-09-19 04:15_

Skips check when slice is of the form `text[a:b:c]` in the case where `c` is not `None`, `True`, or 1.

Closes #13349

---

_Comment by @github-actions[bot] on 2024-09-19 04:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-09-19 04:32_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:262 on 2024-09-19 04:32_

I think this should work:

```suggestion
            }) => x.as_u8() != Some(1),
```

---

_@dylwil3 reviewed on 2024-09-19 05:02_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:262 on 2024-09-19 05:02_

much better! Done.

---

_Label `rule` added by @MichaReiser on 2024-09-19 07:37_

---

_Label `preview` added by @MichaReiser on 2024-09-19 07:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:257 on 2024-09-19 07:40_

Nit
```suggestion
        .as_deref()
        // present and
        .is_some_and(|step| match step {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:268 on 2024-09-19 07:41_

Nit: Use `..` so that this code doesn't need updating when we add a new field to `ExprNumberLiteral` or `BooleanLiteral`
```suggestion
            ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
                value: ast::Number::Int(x),
                ..
            }) => x.as_u8() != Some(1),
            // and not equal to `None` or `True`
            ast::Expr::NoneLiteral(_)
            | ast::Expr::BooleanLiteral(ast::ExprBooleanLiteral {
                value: true,
                ..
            }) => false,
```

---

_@MichaReiser approved on 2024-09-19 07:43_

Thanks!

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:257 on 2024-09-19 13:03_

Done!

---

_@dylwil3 reviewed on 2024-09-19 13:03_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:268 on 2024-09-19 13:03_

Done!

---

_@dylwil3 reviewed on 2024-09-19 13:03_

---

_@AlexWaygood approved on 2024-09-19 16:47_

Thanks!

---

_Merged by @AlexWaygood on 2024-09-19 16:47_

---

_Closed by @AlexWaygood on 2024-09-19 16:47_

---

_Branch deleted on 2024-09-19 16:48_

---
