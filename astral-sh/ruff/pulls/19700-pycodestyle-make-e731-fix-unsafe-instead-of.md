```yaml
number: 19700
title: "[`pycodestyle`] Make `E731` fix unsafe instead of display-only for class assignments"
type: pull_request
state: merged
author: danparizher
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix-19650
created_at: 2025-08-01T22:17:08Z
updated_at: 2025-08-15T19:16:34Z
url: https://github.com/astral-sh/ruff/pull/19700
synced_at: 2026-01-12T15:56:45Z
```

# [`pycodestyle`] Make `E731` fix unsafe instead of display-only for class assignments

---

_@danparizher_

## Summary

Fixes #19650


---

_Comment by @github-actions[bot] on 2025-08-01 22:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-08-08 14:08_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs`:108 on 2025-08-08 19:08_

I don't think the parenthetical `annotation shadowing` helps much compared to the original comment, and it confused me for a second until I read the linked issue. I'd probably leave that out.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs`:115 on 2025-08-08 19:11_

While we're here, we could also switch to `Fix::applicable_edit`, if you want:


```rust
       let applicability = if checker
            .semantic()
            .current_scope()
            .get_all(id)
            .any(|binding_id| checker.semantic().binding(binding_id).kind.is_annotation()) {
                Applicability::DisplayOnly
            } else {
                Applicability::Unsafe
            };

        diagnostic.set_fix(Fix::applicable_edit(...));
```

---

_@ntBre approved on 2025-08-08 19:13_

Looks good, thanks! Just one docs nit and a tangentially-related refactor.

---

_Label `fixes` added by @ntBre on 2025-08-15 18:59_

---

_Merged by @ntBre on 2025-08-15 19:09_

---

_Closed by @ntBre on 2025-08-15 19:09_

---

_Branch deleted on 2025-08-15 19:12_

---
