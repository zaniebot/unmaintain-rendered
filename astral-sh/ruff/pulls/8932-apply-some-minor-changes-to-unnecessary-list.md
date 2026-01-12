```yaml
number: 8932
title: "Apply some minor changes to `unnecessary-list-index-lookup`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/cleanup
created_at: 2023-12-01T00:39:23Z
updated_at: 2023-12-01T00:59:06Z
url: https://github.com/astral-sh/ruff/pull/8932
synced_at: 2026-01-10T23:40:55Z
```

# Apply some minor changes to `unnecessary-list-index-lookup`

---

_Pull request opened by @charliermarsh on 2023-12-01 00:39_

## Summary

I was late in reviewing this but found a few things I wanted to tweak. No functional changes.

---

_@charliermarsh reviewed on 2023-12-01 00:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:18 on 2023-12-01 00:39_

I try to wrap these at 80 characters so that they render reasonably in the terminal.

---

_@charliermarsh reviewed on 2023-12-01 00:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:116 on 2023-12-01 00:40_

I prefer to put the rule implementations up top, and helper functions at the bottom. That way, each file reads as (1) diagnostic definition, then (2) rule body.

---

_@charliermarsh reviewed on 2023-12-01 00:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:84 on 2023-12-01 00:40_

I made this an associated method.

---

_@charliermarsh reviewed on 2023-12-01 00:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:157 on 2023-12-01 00:40_

This is a repeat of `is_assignment`.

---

_@charliermarsh reviewed on 2023-12-01 00:42_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:263 on 2023-12-01 00:42_

I rewrote this as:

```rust
    // Check that the function is the `enumerate` builtin.
    if !semantic
        .resolve_call_path(func.as_ref())
        .is_some_and(|call_path| matches!(call_path.as_slice(), ["builtins" | "", "enumerate"]))
    {
        return None;
    }
```

Note, however, that you could also do `let call_path = checker.semantic().resolve_call_path(func.as_ref())?` here, which is more idiomatic than `let`-`else` with a `return None`.

---

_@charliermarsh reviewed on 2023-12-01 00:42_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:191 on 2023-12-01 00:42_

For a single branch match, I tend to prefer `let`-`else` or `if`-`let`.

---

_@charliermarsh reviewed on 2023-12-01 00:43_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_list_index_lookup.rs`:122 on 2023-12-01 00:43_

Able to avoid the clone that was here before by using a lifetime.

---

_Label `internal` added by @charliermarsh on 2023-12-01 00:43_

---

_Comment by @diceroll123 on 2023-12-01 00:49_

I'll learn from all this, thanks! 😄 

---

_Comment by @charliermarsh on 2023-12-01 00:52_

Of course! Sorry that it took it getting merged for me to actually prioritize reviewing, that's a bad habit 😬 

---

_Merged by @charliermarsh on 2023-12-01 00:53_

---

_Closed by @charliermarsh on 2023-12-01 00:53_

---

_Branch deleted on 2023-12-01 00:53_

---

_Comment by @github-actions[bot] on 2023-12-01 00:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
