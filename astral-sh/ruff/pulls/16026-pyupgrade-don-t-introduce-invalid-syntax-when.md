```yaml
number: 16026
title: "[`pyupgrade`] Don't introduce invalid syntax when upgrading old-style type aliases with parenthesized multiline values (`UP040`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - fixes
  - python312
assignees: []
merged: true
base: main
head: alex/up040
created_at: 2025-02-07T16:36:40Z
updated_at: 2025-02-07T17:07:49Z
url: https://github.com/astral-sh/ruff/pull/16026
synced_at: 2026-01-12T15:55:53Z
```

# [`pyupgrade`] Don't introduce invalid syntax when upgrading old-style type aliases with parenthesized multiline values (`UP040`)

---

_@AlexWaygood_

## Summary

We previously autofixed this:

```py
from typing import TypeAlias

T: TypeAlias = (
    int
    | str
)
```

to this:

```py
type T = int
    | str
```

When we should be autofixing it to this:

```py
type T = (
    int
    | str
)
```

This PR fixes the bug. Closes #16023

## Test Plan

New fixture added that repros the issue on `main`.


---

_Label `bug` added by @AlexWaygood on 2025-02-07 16:36_

---

_Label `fixes` added by @AlexWaygood on 2025-02-07 16:36_

---

_Label `python312` added by @AlexWaygood on 2025-02-07 16:36_

---

_Renamed from "add failing test" to "[`pyupgrade`] Don't introduce invalid syntax when upgrading old-style type aliases (`UP040`)" by @AlexWaygood on 2025-02-07 16:37_

---

_Renamed from "[`pyupgrade`] Don't introduce invalid syntax when upgrading old-style type aliases (`UP040`)" to "[`pyupgrade`] Don't introduce invalid syntax when upgrading old-style type aliases with parenthesized multiline values (`UP040`)" by @AlexWaygood on 2025-02-07 16:37_

---

_Comment by @AlexWaygood on 2025-02-07 16:40_

I think this is a small regression from https://github.com/astral-sh/ruff/pull/15840 (cc. @ntBre) -- totally one I should have caught in review, though!

---

_Comment by @github-actions[bot] on 2025-02-07 16:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:265 on 2025-02-07 16:43_

I'd prefer using `&Stmt`a here if we always have a `stmt`. It encodes the constraint stronger (It otherwise leaves me wondering if `stmt` is guaranteed to be a `stmt` or if it is a left over from a refactor

---

_@MichaReiser approved on 2025-02-07 16:44_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:265 on 2025-02-07 16:46_

> I'd prefer using `&Stmt`a here if we always have a `stmt`.

Unfortunately we don't. This function is called from two possible functions: in one function we have a `&StmtAssign` and in the other we have a `&StmtAnnAssign`.

I could use `StmtRef`, though, if you'd prefer that?

```diff
--- a/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs
+++ b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs
@@ -5,7 +5,7 @@ use ruff_macros::{derive_message_formats, ViolationMetadata};
 use ruff_python_ast::name::Name;
 use ruff_python_ast::parenthesize::parenthesized_range;
 use ruff_python_ast::visitor::Visitor;
-use ruff_python_ast::{AnyNodeRef, Expr, ExprCall, ExprName, Keyword, StmtAnnAssign, StmtAssign};
+use ruff_python_ast::{Expr, ExprCall, ExprName, Keyword, StmtAnnAssign, StmtAssign, StmtRef};
 use ruff_text_size::{Ranged, TextRange};
 
 use crate::checkers::ast::Checker;
@@ -262,7 +262,7 @@ pub(crate) fn non_pep695_type_alias(checker: &Checker, stmt: &StmtAnnAssign) {
 /// Generate a [`Diagnostic`] for a non-PEP 695 type alias or type alias type.
 fn create_diagnostic(
     checker: &Checker,
-    stmt: AnyNodeRef,
+    stmt: StmtRef,
     name: &Name,
     value: &Expr,
     type_vars: &[TypeVar],
@@ -273,8 +273,13 @@ fn create_diagnostic(
     let content = format!(
         "type {name}{type_params} = {value}",
         type_params = DisplayTypeVars { type_vars, source },
-        value = &source[parenthesized_range(value.into(), stmt, checker.comment_ranges(), source)
-            .unwrap_or(value.range())]
+        value = &source[parenthesized_range(
+            value.into(),
+            stmt.into(),
+            checker.comment_ranges(),
+            source
+        )
+        .unwrap_or(value.range())]
     );
```

---

_@AlexWaygood reviewed on 2025-02-07 16:46_

---

_@InSyncWithFoo reviewed on 2025-02-07 16:49_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP040.py`:120 on 2025-02-07 16:49_

This might be a good test case to add:

```python
T: TypeAlias = (  # This comment should not make the fix unsafe
	int | str
)
```

---

_@AlexWaygood reviewed on 2025-02-07 16:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP040.py`:120 on 2025-02-07 16:53_

Thanks! However, all fixes for this rule are unsafe (because you can't use PEP-695 type aliases as the second argument in `isinstance()` calls, whereas you can with old-style type aliases that use `TypeAlias`)

---

_@InSyncWithFoo reviewed on 2025-02-07 16:55_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP040.py`:120 on 2025-02-07 16:55_

> However, all fixes for this rule are unsafe

Aren't they safe in stubs?

---

_Comment by @ntBre on 2025-02-07 16:55_

Sorry about that! Thanks for the quick fix! I don't have anything to add on top of the other reviews, LGTM.

---

_@AlexWaygood reviewed on 2025-02-07 16:57_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP040.py`:120 on 2025-02-07 16:57_

> Aren't they safe in stubs?

hmm, good point. Want to make a followup PR?

---

_@AlexWaygood reviewed on 2025-02-07 16:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:265 on 2025-02-07 16:59_

I switched to use `StmtRef`

---

_@InSyncWithFoo reviewed on 2025-02-07 17:01_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP040.py`:120 on 2025-02-07 17:01_

Sure. I'm ready whenever you are.

---

_Merged by @AlexWaygood on 2025-02-07 17:05_

---

_Closed by @AlexWaygood on 2025-02-07 17:05_

---

_Branch deleted on 2025-02-07 17:05_

---
