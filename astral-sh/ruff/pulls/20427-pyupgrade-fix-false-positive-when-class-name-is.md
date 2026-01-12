```yaml
number: 20427
title: "[`pyupgrade`] Fix false positive when class name is shadowed by local variable (`UP008`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-20422
created_at: 2025-09-15T23:44:07Z
updated_at: 2025-09-18T14:12:38Z
url: https://github.com/astral-sh/ruff/pull/20427
synced_at: 2026-01-12T15:57:01Z
```

# [`pyupgrade`] Fix false positive when class name is shadowed by local variable (`UP008`)

---

_@danparizher_

## Summary

Fixes #20422


---

_Comment by @github-actions[bot] on 2025-09-15 23:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:155 on 2025-09-17 19:15_

Can we combine this with the other `first_arg_id == parent_name.as_str()` check to avoid having to check that twice?

```rust
    if !((first_arg_id == "__class__"
        || (first_arg_id == parent_name.as_str()
            && !checker.semantic().current_scope().has(first_arg_id)))
        && second_arg_id == parent_arg.name().as_str())
    {
        return;
    }
```

This condition is definitely getting a bit unwieldy, but I don't think it's too much worse.

An alternative approach here would be to resolve the binding of the first argument name and compare that to the binding of the parent class, but I think this check is okay because any intervening scope should also disable the rule. The current approach is a bit simpler.

---

_@ntBre reviewed on 2025-09-17 19:26_

Thanks! I just had a minor simplification suggestion, but this looks good to me.

---

_Label `bug` added by @ntBre on 2025-09-17 19:26_

---

_Label `rule` added by @ntBre on 2025-09-17 19:26_

---

_Review requested from @ntBre by @danparizher on 2025-09-17 22:51_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:146 on 2025-09-18 14:01_

```suggestion
            // If the first argument matches the class name, check if it's a local variable
            // that shadows the class name. If so, don't apply UP008.
            && !checker.semantic().current_scope().has(first_arg_id)))
```

Just adding back your comment, we'll see if I got the formatting right in the `suggestion` box!

---

_@ntBre approved on 2025-09-18 14:01_

Thank you!

---

_Merged by @ntBre on 2025-09-18 14:05_

---

_Closed by @ntBre on 2025-09-18 14:05_

---

_Branch deleted on 2025-09-18 14:10_

---
