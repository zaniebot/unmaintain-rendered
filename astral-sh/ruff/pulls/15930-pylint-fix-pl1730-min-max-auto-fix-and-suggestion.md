```yaml
number: 15930
title: "[pylint] Fix PL1730: min/max auto-fix and suggestion"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-PLR1730-autofix-and-suggestion
created_at: 2025-02-04T11:49:24Z
updated_at: 2025-02-05T09:31:45Z
url: https://github.com/astral-sh/ruff/pull/15930
synced_at: 2026-01-10T19:57:22Z
```

# [pylint] Fix PL1730: min/max auto-fix and suggestion

---

_Pull request opened by @VascoSch92 on 2025-02-04 11:49_

## Summary

The PR addresses the issue #15887 

For two objects `a` and `b`, we ensure that the auto-fix and the suggestion is of the form `a = min(a, b)` (or `a = max(a, b)`). This is because we want to be consistent with the python implementation of the methods: `min` and `max`. See the above issue for more details.






---

_Comment by @github-actions[bot] on 2025-02-04 11:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @MichaReiser on 2025-02-04 13:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/if_stmt_min_max.rs`:178 on 2025-02-04 13:45_

Nit
```suggestion
    let first_cmp_arg: &str;
    let second_cmp_arg: &str;
    let (first_cmp_arg, second_cmp_arg) = if checker.locator().slice(arg1) == cmp_target {
      (
    	  	checker.locator().slice(arg1),
        	checker.locator().slice(arg2),
    	)
    } else {
    	(
        checker.locator().slice(arg2),
        checker.locator().slice(arg1),
      )
    };
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/if_stmt_min_max.rs`:172 on 2025-02-04 13:47_

Can we reuse `flip_args` from above or the `left_is_target`/`right_is_target` variables?

---

_@MichaReiser reviewed on 2025-02-04 13:47_

---

_Comment by @VascoSch92 on 2025-02-04 15:50_

@MichaReiser I implemented your suggestion ;-) Let me know if you like it...

---

_Review requested from @MichaReiser by @VascoSch92 on 2025-02-04 16:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/if_stmt_min_max.rs`:151 on 2025-02-05 08:28_

I think we can simplify this to (and also reduce the diff size):

```rust
    let min_max = match (
        left_is_target,
        right_is_target,
        left_is_value,
        right_is_value,
    ) {
        (true, false, false, true) => match op {
            CmpOp::Lt | CmpOp::LtE => MinMax::Max,
            CmpOp::Gt | CmpOp::GtE => MinMax::Min,
            _ => return,
        },
        (false, true, true, false) => match op {
            CmpOp::Lt | CmpOp::LtE => MinMax::Min,
            CmpOp::Gt | CmpOp::GtE => MinMax::Max,
            _ => return,
        },
        _ => return,
    };

		// Determine whether to use `min()` or `max()`, and make sure that the first
    // arg of the `min()` or `max()` method is equal to the target of the comparison.
    // This is to be consistent with the Python implementation of the methods `min()` and `max()`.
    let (arg1, arg2) = if left_is_target {
        (&**left, right)
    } else {
        (right, &**left)
    };
```

---

_@MichaReiser reviewed on 2025-02-05 08:28_

Thanks. I think we can simplify this further. 

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/pylint/rules/if_stmt_min_max.rs`:151 on 2025-02-05 09:24_

Yes... I did the changes ;-)

---

_@VascoSch92 reviewed on 2025-02-05 09:24_

---

_@MichaReiser approved on 2025-02-05 09:27_

Nice, thank you

---

_Label `bug` added by @MichaReiser on 2025-02-05 09:27_

---

_Merged by @MichaReiser on 2025-02-05 09:29_

---

_Closed by @MichaReiser on 2025-02-05 09:29_

---

_Branch deleted on 2025-02-05 09:31_

---
