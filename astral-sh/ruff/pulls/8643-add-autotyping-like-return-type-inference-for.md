```yaml
number: 8643
title: Add autotyping-like return type inference for annotation rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/autotyping
created_at: 2023-11-13T04:40:08Z
updated_at: 2023-11-14T04:44:16Z
url: https://github.com/astral-sh/ruff/pull/8643
synced_at: 2026-01-12T15:55:26Z
```

# Add autotyping-like return type inference for annotation rules

---

_@charliermarsh_

## Summary

This PR adds (unsafe) fixes to the flake8-annotations rules that enforce missing return types, offering to automatically insert type annotations for functions with literal return values. The logic is smart enough to generate simplified unions (e.g., `float` instead of `int | float`) and deal with implicit returns (`return` without a value).

Closes https://github.com/astral-sh/ruff/issues/1640 (though we could open a separate issue for referring parameter types).

Closes https://github.com/astral-sh/ruff/issues/8213.

## Test Plan

`cargo test`


---

_Review requested from @zanieb by @charliermarsh on 2023-11-13 04:44_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-11-13 04:44_

---

_Label `autofix` added by @charliermarsh on 2023-11-13 04:44_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:231 on 2023-11-13 04:49_

nit: for consistency with the violation message. Similar suggestion for other fix titles.
```suggestion
    fn fix_title(&self) -> Option<String> {
        let Self { annotation, .. } = self;
        if let Some(annotation) = annotation {
            Some(format!("Add return type annotation: `{annotation}`"))
        } else {
            Some(format!("Add return type annotation"))
        }
    }
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/helpers.rs`:1262 on 2023-11-13 04:58_

nit: Can we name this something more specific now that it's a public helper function? Maybe `into_pep_604_optional` / `as_pep_604_optional`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/analyze/type_inference.rs`:61 on 2023-11-13 05:12_

I think we would need to have different variable names here for `type_` like `type_a`, `type_b`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:81 on 2023-11-13 05:18_

nit: not critical but maybe we could update `union` to take an iterator instead to avoid allocating a vector here

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:83 on 2023-11-13 05:21_

I think it's fine but is there any specific reason we're not implementing the `typing.Union` variant for pre 3.10?

---

_@dhruvmanila reviewed on 2023-11-13 05:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:83 on 2023-11-13 14:49_

Just complexity and/or laziness.

---

_@charliermarsh reviewed on 2023-11-13 14:49_

---

_Comment by @github-actions[bot] on 2023-11-13 17:28_

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

_Review requested from @dhruvmanila by @charliermarsh on 2023-11-13 17:37_

---

_@dhruvmanila reviewed on 2023-11-14 04:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/analyze/type_inference.rs`:54 on 2023-11-14 04:19_

```suggestion
                    // If `b_element` is a subtype of any of the types in `a`, then
                    // `b_element` is redundant.
```

---

_@dhruvmanila approved on 2023-11-14 04:19_

---

_Merged by @charliermarsh on 2023-11-14 04:34_

---

_Closed by @charliermarsh on 2023-11-14 04:34_

---

_Branch deleted on 2023-11-14 04:34_

---
