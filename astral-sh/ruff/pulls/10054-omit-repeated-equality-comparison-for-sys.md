```yaml
number: 10054
title: Omit repeated equality comparison for sys
type: pull_request
state: merged
author: arjunnn
labels:
  - bug
assignees: []
merged: true
base: main
head: omit-repeated-equality-comparison-for-sys
created_at: 2024-02-19T23:53:55Z
updated_at: 2024-02-20T22:10:22Z
url: https://github.com/astral-sh/ruff/pull/10054
synced_at: 2026-01-12T15:55:31Z
```

# Omit repeated equality comparison for sys

---

_@arjunnn_

## Summary
Update PLR1714 to ignore `sys.platform` and `sys.version` checks. 
I'm not sure if these checks or if we need to add more. Please advise.

Fixes #10017

## Test Plan
Added a new test case and ran `cargo nextest run`

---

_@charliermarsh reviewed on 2024-02-19 23:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs`:209 on 2024-02-19 23:59_

Thanks! Can you try something like:

```rust
    any_over_expr(test, &|expr| {
        semantic.resolve_call_path(expr).is_some_and(|call_path| {
            matches!(call_path.as_slice(), ["sys", "version_info" | "platform"])
        })
    })
```

This is roughly taken from a helper we have called `is_sys_version_block`.

---

_@charliermarsh requested changes on 2024-02-20 16:03_

Just putting this back in your queue :)

---

_@charliermarsh approved on 2024-02-20 18:56_

---

_Label `bug` added by @charliermarsh on 2024-02-20 18:56_

---

_Merged by @charliermarsh on 2024-02-20 19:03_

---

_Closed by @charliermarsh on 2024-02-20 19:03_

---

_Comment by @github-actions[bot] on 2024-02-20 19:16_

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

_Comment by @arjunnn on 2024-02-20 22:08_

Sorry, I couldn't get to this in time. Thank you!

---

_Comment by @charliermarsh on 2024-02-20 22:10_

No prob, I was just in PR-closing-mode and took advantage of being in the area. Thanks for contributing!

---
