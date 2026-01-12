```yaml
number: 7841
title: Add settings for promoting and demoting fixes
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
assignees: []
merged: true
base: main
head: zanie/app-settings
created_at: 2023-10-06T19:46:00Z
updated_at: 2023-10-11T12:04:32Z
url: https://github.com/astral-sh/ruff/pull/7841
synced_at: 2026-01-12T02:32:41Z
```

# Add settings for promoting and demoting fixes

---

_Pull request opened by @zanieb on 2023-10-06 19:46_

Adds two configuration-file only settings `extend-safe-fixes` and `extend-unsafe-fixes` which can be used to promote and demote the applicability of fixes for rules.

Fixes with `Never` applicability cannot be promoted.

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:273 on 2023-10-06 19:47_

Does this clone matter? Is there a better way to do this?

---

_@zanieb reviewed on 2023-10-06 19:47_

---

_@zanieb reviewed on 2023-10-06 19:54_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:1221 on 2023-10-06 19:54_

I copied this testing strategy from the formatter integration tests. Do we have a better place to do this? I'd test more cases (e.g. prefixes, rules in both sets, etc.)

---

_Comment by @github-actions[bot] on 2023-10-06 21:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @konstin on `crates/ruff_linter/src/linter.rs`:273 on 2023-10-09 09:36_

This would work:
```rust
            if let Some(fix) = diagnostic.fix.take() {
                // Check unsafe before safe so if someone puts a rule in both we are conservative
                if settings
                    .extend_unsafe_fixes
                    .contains(diagnostic.kind.rule())
                    && fix.applicability().is_always()
                {
                    diagnostic.set_fix(fix.with_applicability(Applicability::Sometimes));
                } else if settings.extend_safe_fixes.contains(diagnostic.kind.rule())
                    && fix.applicability().is_sometimes()
                {
                    diagnostic.set_fix(fix.with_applicability(Applicability::Always));
                } else {
                    diagnostic.set_fix(fix);
                }
            }
```
There's no `Fix::set_applicability(&mut self, ...)` (only a `mut self` method), so usual the `if let Some(fix) = &mut diagnostic.fix` trick doesn't work here

---

_@konstin reviewed on 2023-10-09 09:36_

---

_@zanieb reviewed on 2023-10-10 16:11_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:273 on 2023-10-10 16:11_

Thanks!

Should I have made `set_applicability` take `&mut self`?

---

_Marked ready for review by @zanieb on 2023-10-10 16:12_

---

_Label `configuration` added by @zanieb on 2023-10-10 17:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/linter.rs`:271 on 2023-10-10 18:05_

I think it's worth inverting the order of the `&&` here, since the second condition is much cheaper.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/linter.rs`:280 on 2023-10-10 18:10_

I think you could also write this section like:

```rust
if let Some(fix) = &mut diagnostic.fix {
    // Check unsafe before safe so if someone puts a rule in both we are conservative
    if settings
        .extend_unsafe_fixes
        .contains(diagnostic.kind.rule())
        && fix.applicability().is_safe()
    {
        fix.applicability = Applicability::Unsafe;
    } else if settings.extend_safe_fixes.contains(diagnostic.kind.rule())
        && fix.applicability().is_unsafe()
    {
        fix.applicability = Applicability::Safe;
    }
}
```

If `applicability` is `pub(crate)`, or more likely with something like `set_applicability(&mut self, applicability: Applicability)` on `Fix`. But I think I prefer what you have.

---

_@charliermarsh approved on 2023-10-10 18:10_

---

_@zanieb reviewed on 2023-10-10 18:29_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:271 on 2023-10-10 18:29_

Hm I thought I was being careful to do the cheaper option first but it sure doesn't look like it!

---

_@zanieb reviewed on 2023-10-10 19:57_

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:280 on 2023-10-10 19:57_

Thanks for explaining!

---

_Merged by @zanieb on 2023-10-10 20:04_

---

_Closed by @zanieb on 2023-10-10 20:04_

---

_Branch deleted on 2023-10-10 20:04_

---

_@konstin reviewed on 2023-10-11 12:04_

---

_Review comment by @konstin on `crates/ruff_linter/src/linter.rs`:280 on 2023-10-11 12:04_

To add one more alternative:
```rust
            diagnostic.fix = diagnostic.fix.take().map(|fix| {
                // Enforce demotions over promotions so if someone puts a rule in both we are conservative
                if fix.applicability().is_safe()
                    && settings
                        .extend_unsafe_fixes
                        .contains(diagnostic.kind.rule())
                {
                    fix.with_applicability(Applicability::Unsafe)
                } else if fix.applicability().is_unsafe()
                    && settings.extend_safe_fixes.contains(diagnostic.kind.rule())
                {
                    fix.with_applicability(Applicability::Safe)
                } else {
                    // Retain the existing fix (will be dropped from `.take()` otherwise)
                    fix
                }
            });
```
(it's annoying that the compiler doesn't accept this without `.take()`)

---
