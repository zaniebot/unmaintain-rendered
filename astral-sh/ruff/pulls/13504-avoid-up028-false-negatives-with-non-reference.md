```yaml
number: 13504
title: Avoid UP028 false negatives with non-reference shadowed bindings of loop variables
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/up028-shadow
created_at: 2024-09-24T14:44:30Z
updated_at: 2024-09-25T15:03:12Z
url: https://github.com/astral-sh/ruff/pull/13504
synced_at: 2026-01-12T15:55:44Z
```

# Avoid UP028 false negatives with non-reference shadowed bindings of loop variables

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/13266

Avoids false negatives for shadowed bindings that aren't actually references to the loop variable. There are some shadowed bindings we need to support still, e.g., `del` requires the loop variable to exist.

---

_Label `bug` added by @zanieb on 2024-09-24 14:44_

---

_@zanieb reviewed on 2024-09-24 14:45_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP028_1.py.snap`:24 on 2024-09-24 14:45_

This is a regression we will need to fix before merging.

---

_@zanieb reviewed on 2024-09-24 14:46_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyupgrade/rules/yield_in_for_loop.rs`:103 on 2024-09-24 14:46_

This was retrieving all shadowed bindings matching the name too, which seems way overkill and leads to false negatives.

---

_Comment by @github-actions[bot] on 2024-09-24 14:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @zanieb on 2024-09-24 16:17_

---

_Review requested from @charliermarsh by @zanieb on 2024-09-24 16:17_

---

_Renamed from "Avoid reading shadowed bindings of loop variables when considering if UP028 is safe" to "Avoid reading non-reference shadowed bindings of loop variables when considering if UP028 is safe" by @zanieb on 2024-09-24 16:18_

---

_Renamed from "Avoid reading non-reference shadowed bindings of loop variables when considering if UP028 is safe" to "Avoid UP028 false negatives with non-reference shadowed bindings of loop variables" by @zanieb on 2024-09-24 16:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/yield_in_for_loop.rs`:125 on 2024-09-25 07:24_

Nit: You could avoid rolling your own shadow-binding traversal by keeping `get_all` and use `find`

```rust
        checker
            .semantic()
            .current_scope()
            .get_all(&name.id)
            // If the binding is an unbound kind shadowing another binding, resolve it to the
            // shadowed binding.
            .find(|&id| !checker.semantic().binding(id).is_unbound())
            .map(|binding_id| {
                let binding = checker.semantic().binding(binding_id);
                binding.references.iter().any(|reference_id| {
                    checker.semantic().reference(*reference_id).range() != name.range()
                })
            })
            .unwrap_or_default()
```

---

_@MichaReiser approved on 2024-09-25 07:25_

Thanks, this looks good to me but I also know very little about our bindings infrastructure. 

---

_@zanieb reviewed on 2024-09-25 13:31_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyupgrade/rules/yield_in_for_loop.rs`:125 on 2024-09-25 13:31_

Thanks! Not sure why I thought that was different.

---

_Merged by @zanieb on 2024-09-25 15:03_

---

_Closed by @zanieb on 2024-09-25 15:03_

---

_Branch deleted on 2024-09-25 15:03_

---
