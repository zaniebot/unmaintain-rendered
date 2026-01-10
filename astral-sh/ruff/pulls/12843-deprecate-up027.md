```yaml
number: 12843
title: "Deprecate `UP027`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
  - breaking
assignees: []
merged: true
base: ruff-0.6
head: deprecate-up027
created_at: 2024-08-12T11:59:07Z
updated_at: 2024-08-12T12:34:14Z
url: https://github.com/astral-sh/ruff/pull/12843
synced_at: 2026-01-10T21:38:32Z
```

# Deprecate `UP027`

---

_Pull request opened by @MichaReiser on 2024-08-12 11:59_

## Summary
Closes https://github.com/astral-sh/ruff/issues/12754

## Test Plan

```shell
cargo run --bin ruff -- check --select UP027 ../ 
warning: Rule `UP027` is deprecated and will be removed in a future release.
warning: Detected debug build without --no-cache.
ruff failed
  Cause: Selection of deprecated rule `UP027` is not allowed when preview is enabled.
```


---

_Label `rule` added by @MichaReiser on 2024-08-12 12:00_

---

_Label `breaking` added by @MichaReiser on 2024-08-12 12:00_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/unpacked_list_comprehension.rs`:1 on 2024-08-12 12:02_

I'd also tweak the "Why is this bad?" paragraph lower down:

```diff
--- a/crates/ruff_linter/src/rules/pyupgrade/rules/unpacked_list_comprehension.rs
+++ b/crates/ruff_linter/src/rules/pyupgrade/rules/unpacked_list_comprehension.rs
@@ -16,8 +16,7 @@ use crate::checkers::ast::Checker;
 ///
 /// ## Why is this bad?
 /// There is no reason to use a list comprehension if the result is immediately
-/// unpacked. Instead, use a generator expression, which is more efficient as
-/// it avoids allocating an intermediary list.
+/// unpacked. Instead, use a generator expression, which avoids allocating an intermediary list.
 ///
 /// ## Example
 /// ```python
```

---

_@AlexWaygood approved on 2024-08-12 12:02_

---

_@AlexWaygood reviewed on 2024-08-12 12:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/unpacked_list_comprehension.rs`:19 on 2024-08-12 12:05_

```suggestion
/// unpacked. Instead, use a generator expression, which avoids allocating
```

---

_@AlexWaygood approved on 2024-08-12 12:14_

---

_Comment by @github-actions[bot] on 2024-08-12 12:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-08-12 12:34_

---

_Closed by @MichaReiser on 2024-08-12 12:34_

---

_Branch deleted on 2024-08-12 12:34_

---
