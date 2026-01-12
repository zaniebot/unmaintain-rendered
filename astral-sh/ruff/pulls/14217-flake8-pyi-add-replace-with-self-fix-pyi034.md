```yaml
number: 14217
title: "[`flake8-pyi`] Add \"replace with `Self`\" fix (`PYI034`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - fixes
assignees: []
merged: true
base: main
head: PYI034
created_at: 2024-11-09T00:03:36Z
updated_at: 2024-11-15T10:55:05Z
url: https://github.com/astral-sh/ruff/pull/14217
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-pyi`] Add "replace with `Self`" fix (`PYI034`)

---

_@InSyncWithFoo_

## Summary

Resolves #14184.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-11-09 00:03_

---

_Comment by @github-actions[bot] on 2024-11-09 00:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-11-09 01:55_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:109 on 2024-11-09 01:55_

```suggestion
    let source_module = if checker.source_type.is_stub() || target_version >= (3, 11) {
        "typing"
    } else {
        "typing_extensions"
```

---

_@charliermarsh reviewed on 2024-11-09 01:56_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:109 on 2024-11-09 01:56_

I think we _do_ want to use `typing` in stubs.

---

_@charliermarsh approved on 2024-11-09 02:07_

Thanks!

---

_Merged by @charliermarsh on 2024-11-09 02:11_

---

_Closed by @charliermarsh on 2024-11-09 02:11_

---

_@InSyncWithFoo reviewed on 2024-11-09 02:12_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:109 on 2024-11-09 02:12_

I don't think `Self` can be imported from `typing` on 3.10 and lower, even in stubs. How about `if target_version >= (3, 11) { "typing" } else { "typing_extensions" }`?

---

_@AlexWaygood reviewed on 2024-11-09 11:09_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:109 on 2024-11-09 11:09_

@InSyncWithFoo is correct -- `Self` cannot be imported from `typing` in a `.py` file or a `.pyi` file if you want the code to support Python 3.10 or lower (see also: https://github.com/astral-sh/ruff/issues/9761).

In typeshed we always import `Self` from `typing_extensions`, because typeshed pretends that `typing_extensions` is part of the stdlib, and we want our stubs to support Python 3.8-3.13.

---

_@charliermarsh reviewed on 2024-11-09 13:48_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:109 on 2024-11-09 13:48_

Thanks both! I assumed the semantics were similar to PEP 585.

---

_Branch deleted on 2024-11-10 04:36_

---

_Label `fixes` added by @dhruvmanila on 2024-11-15 10:55_

---
