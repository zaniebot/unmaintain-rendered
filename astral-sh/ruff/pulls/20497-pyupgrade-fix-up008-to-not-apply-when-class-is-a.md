```yaml
number: 20497
title: "[`pyupgrade`] Fix `UP008` to not apply when `__class__` is a local variable (`UP008`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-20491
created_at: 2025-09-21T23:17:42Z
updated_at: 2025-09-23T15:03:09Z
url: https://github.com/astral-sh/ruff/pull/20497
synced_at: 2026-01-10T17:40:28Z
```

# [`pyupgrade`] Fix `UP008` to not apply when `__class__` is a local variable (`UP008`)

---

_Pull request opened by @danparizher on 2025-09-21 23:17_

## Summary

Fixes #20491


---

_Comment by @github-actions[bot] on 2025-09-21 23:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-09-23 14:15_

---

_@ntBre approved on 2025-09-23 14:16_

Thank you!

---

_@ntBre reviewed on 2025-09-23 14:25_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:143 on 2025-09-23 14:25_

Actually, I think we can now refactor this condition to

```rust
    if !((first_arg_id == "__class__" || first_arg_id == parent_name.as_str())
        && !checker.semantic().current_scope().has(first_arg_id)
        && second_arg_id == parent_arg.name().as_str())
```

Since `first_arg_id` will equal `__class__` in that case, we can just reuse the `has(first_arg_id)` check, unless I'm missing something.

I dropped the comments for convenience while editing this, but I think it's nice to have them here, possibly combining them if the check is combined too.

---

_Review requested from @ntBre by @danparizher on 2025-09-23 14:43_

---

_@ntBre approved on 2025-09-23 14:47_

---

_Merged by @ntBre on 2025-09-23 14:56_

---

_Closed by @ntBre on 2025-09-23 14:56_

---

_Branch deleted on 2025-09-23 15:03_

---
