```yaml
number: 20907
title: "[`flake8-django`] Apply `DJ001` to annotated fields"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-20906
created_at: 2025-10-16T00:19:57Z
updated_at: 2025-10-27T14:23:44Z
url: https://github.com/astral-sh/ruff/pull/20907
synced_at: 2026-01-12T15:57:12Z
```

# [`flake8-django`] Apply `DJ001` to annotated fields

---

_@danparizher_

## Summary

Fixes #20906


---

_Comment by @github-actions[bot] on 2025-10-16 00:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_django/rules/nullable_model_string_field.rs`:87 on 2025-10-16 08:25_

We should avoid duplicating that much code. All we need is to extract value. Something like this should work

```suggestion
        let value = match statement {
            Stmt::Assign(ast::StmtAssign { value, .. }) => value,
            Stmt::AnnAssign(ast::StmtAnnAssign {
                value: Some(value), ..
            }) => value,
            _ => continue,
        };
        
        
        if let Some(field_name) = is_nullable_field(value, checker.semantic()) {
            checker.report_diagnostic(
                DjangoNullableModelStringField {
                    field_name: field_name.to_string(),
                },
                value.range(),
            );
        }
```

---

_@MichaReiser reviewed on 2025-10-16 08:25_

---

_@MichaReiser reviewed on 2025-10-16 08:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_django/DJ001.py`:84 on 2025-10-16 08:26_

Do we need all those tests? They seem redundant to me, considering that we ignore the annotation.

---

_Review requested from @MichaReiser by @danparizher on 2025-10-19 20:12_

---

_Renamed from "[`flake8-django`] Fix DJ001 not detecting `null=True` violations in annotated assignments (`DJ001`)" to "[`flake8-django`] Apply `DJ001` to annotated fields" by @ntBre on 2025-10-22 22:45_

---

_Label `bug` added by @ntBre on 2025-10-22 22:45_

---

_@ntBre approved on 2025-10-22 22:46_

Looks good to me, if Micha's happy with it!

---

_Merged by @MichaReiser on 2025-10-27 08:19_

---

_Closed by @MichaReiser on 2025-10-27 08:19_

---

_Branch deleted on 2025-10-27 14:23_

---
