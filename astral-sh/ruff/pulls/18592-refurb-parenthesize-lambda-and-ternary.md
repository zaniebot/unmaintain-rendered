```yaml
number: 18592
title: "[`refurb`] Parenthesize lambda and ternary expressions in iter (`FURB122`, `FURB142`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: brent/fix-furb1
created_at: 2025-06-09T13:52:35Z
updated_at: 2025-06-09T20:07:36Z
url: https://github.com/astral-sh/ruff/pull/18592
synced_at: 2026-01-10T18:45:04Z
```

# [`refurb`] Parenthesize lambda and ternary expressions in iter (`FURB122`, `FURB142`)

---

_Pull request opened by @ntBre on 2025-06-09 13:52_

Summary
--

Fixes #18590 by adding parentheses around lambdas and if expressions in `for` loop iterators for FURB122 and FURB142. I also updated the docs on the helper function to reflect the part actually being parenthesized and the new checks.

The `lambda` case actually causes a `TypeError` at runtime, but I think it's still worth handling to avoid causing a syntax error.

```pycon
>>> s = set()
... for x in (1,) if True else (2,):
...     s.add(-x)
... for x in lambda: 0:
...     s.discard(-x)
...
Traceback (most recent call last):
  File "<python-input-0>", line 4, in <module>
    for x in lambda: 0:
             ^^^^^^^^^
TypeError: 'function' object is not iterable
```

Test Plan
--

New test cases based on the bug report

---

_Label `bug` added by @ntBre on 2025-06-09 13:52_

---

_Label `fixes` added by @ntBre on 2025-06-09 13:52_

---

_Marked ready for review by @ntBre on 2025-06-09 13:57_

---

_Comment by @github-actions[bot] on 2025-06-09 13:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2025-06-09 14:13_

I think we only want to parenthesize if the fix would write out a comprehension and if it's not already parenthesized, right? So maybe we need to do something like this:

```diff
diff --git c/crates/ruff_linter/src/rules/refurb/rules/for_loop_set_mutations.rs w/crates/ruff_linter/src/rules/refurb/rules/for_loop_set_mutations.rs
index 08ccf08d7..84dd5e9a7 100644
--- c/crates/ruff_linter/src/rules/refurb/rules/for_loop_set_mutations.rs
+++ w/crates/ruff_linter/src/rules/refurb/rules/for_loop_set_mutations.rs
@@ -5,7 +5,7 @@ use ruff_python_semantic::analyze::typing;
 use crate::checkers::ast::Checker;
 use crate::{AlwaysFixableViolation, Applicability, Edit, Fix};
 
-use super::helpers::parenthesize_loop_iter_if_necessary;
+use super::helpers::{IterLocation, parenthesize_loop_iter_if_necessary};
 
 /// ## What it does
 /// Checks for code that updates a set with the contents of an iterable by
@@ -106,7 +106,7 @@ pub(crate) fn for_loop_set_mutations(checker: &Checker, for_stmt: &StmtFor) {
             format!(
                 "{}.{batch_method_name}({})",
                 set.id,
-                parenthesize_loop_iter_if_necessary(for_stmt, checker),
+                parenthesize_loop_iter_if_necessary(for_stmt, checker, IterLocation::CallArgument),
             )
         }
         (for_target, arg) => format!(
@@ -114,7 +114,7 @@ pub(crate) fn for_loop_set_mutations(checker: &Checker, for_stmt: &StmtFor) {
             set.id,
             locator.slice(arg),
             locator.slice(for_target),
-            parenthesize_loop_iter_if_necessary(for_stmt, checker),
+            parenthesize_loop_iter_if_necessary(for_stmt, checker, IterLocation::InComprehension),
         ),
     };
 
diff --git c/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs w/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs
index 11ec83337..85180bd33 100644
--- c/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs
+++ w/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs
@@ -5,6 +5,7 @@ use ruff_python_semantic::{Binding, ScopeId, SemanticModel, TypingOnlyBindingsSt
 use ruff_text_size::{Ranged, TextRange, TextSize};
 
 use crate::checkers::ast::Checker;
+use crate::rules::refurb::rules::helpers::IterLocation;
 use crate::{AlwaysFixableViolation, Applicability, Edit, Fix};
 
 use super::helpers::parenthesize_loop_iter_if_necessary;
@@ -182,7 +183,7 @@ fn for_loop_writes(
             format!(
                 "{}.writelines({})",
                 locator.slice(io_object_name),
-                parenthesize_loop_iter_if_necessary(for_stmt, checker),
+                parenthesize_loop_iter_if_necessary(for_stmt, checker, IterLocation::CallArgument),
             )
         }
         (for_target, write_arg) => {
@@ -191,7 +192,11 @@ fn for_loop_writes(
                 locator.slice(io_object_name),
                 locator.slice(write_arg),
                 locator.slice(for_target),
-                parenthesize_loop_iter_if_necessary(for_stmt, checker),
+                parenthesize_loop_iter_if_necessary(
+                    for_stmt,
+                    checker,
+                    IterLocation::InComprehension
+                ),
             )
         }
     };
diff --git c/crates/ruff_linter/src/rules/refurb/rules/helpers.rs w/crates/ruff_linter/src/rules/refurb/rules/helpers.rs
index 8f431ffcc..fa6b4a4d2 100644
--- c/crates/ruff_linter/src/rules/refurb/rules/helpers.rs
+++ w/crates/ruff_linter/src/rules/refurb/rules/helpers.rs
@@ -21,6 +21,7 @@ use crate::checkers::ast::Checker;
 pub(super) fn parenthesize_loop_iter_if_necessary<'a>(
     for_stmt: &'a ast::StmtFor,
     checker: &'a Checker,
+    iter_location: IterLocation,
 ) -> Cow<'a, str> {
     let locator = checker.locator();
     let iter = for_stmt.iter.as_ref();
@@ -42,7 +43,25 @@ pub(super) fn parenthesize_loop_iter_if_necessary<'a>(
         ast::Expr::Tuple(tuple) if !tuple.parenthesized => {
             Cow::Owned(format!("({iter_in_source})"))
         }
-        ast::Expr::Lambda(_) | ast::Expr::If(_) => Cow::Owned(format!("({iter_in_source})")),
+        ast::Expr::If(_) | ast::Expr::Lambda(_)
+            if matches!(iter_location, IterLocation::InComprehension) =>
+        {
+            if let Some(_) = parenthesized_range(
+                iter.into(),
+                for_stmt.into(),
+                checker.comment_ranges(),
+                checker.source(),
+            ) {
+                Cow::Borrowed(iter_in_source)
+            } else {
+                Cow::Owned(format!("({iter_in_source})"))
+            }
+        }
         _ => Cow::Borrowed(iter_in_source),
     }
 }
+
+pub(crate) enum IterLocation {
+    CallArgument,
+    InComprehension,
+}
```

---

_Comment by @dylwil3 on 2025-06-09 14:23_

Incidentally, and unrelated to your PR - in addition to the doc-comment being a little off, we also have two separate helpers modules for refurb rules:

- https://github.com/astral-sh/ruff/blob/ae2150bfa35fe2e4c306f32e1609e76e9ca5520f/crates/ruff_linter/src/rules/refurb/helpers.rs
- https://github.com/astral-sh/ruff/blob/ae2150bfa35fe2e4c306f32e1609e76e9ca5520f/crates/ruff_linter/src/rules/refurb/rules/helpers.rs

maybe they should be merged?

---

_Comment by @ntBre on 2025-06-09 14:27_

I think we will have already bailed out if the range is already parenthesized:

https://github.com/astral-sh/ruff/blob/931577219f894630ef7ce963b7cab3b0ff7a5a65/crates/ruff_linter/src/rules/refurb/rules/helpers.rs#L28-L37

but you're probably right about the callable part. I'll work on a test case for that.

I did also notice the two `helpers` modules. Which way would you merge them? Looks like we have a pretty even mix of `rules/LINTER/helpers.rs` and `rules/LINTER/rules/helpers.rs` :smile: I'd probably lean toward `LINTER/helpers.rs`

```shell
> fd helpers.rs
crates/ruff_linter/src/cst/helpers.rs
crates/ruff_linter/src/rules/airflow/helpers.rs
crates/ruff_linter/src/rules/flake8_2020/helpers.rs
crates/ruff_linter/src/rules/flake8_annotations/helpers.rs
crates/ruff_linter/src/rules/flake8_async/helpers.rs
crates/ruff_linter/src/rules/flake8_bandit/helpers.rs
crates/ruff_linter/src/rules/flake8_boolean_trap/helpers.rs
crates/ruff_linter/src/rules/flake8_bugbear/helpers.rs
crates/ruff_linter/src/rules/flake8_builtins/helpers.rs
crates/ruff_linter/src/rules/flake8_comprehensions/rules/helpers.rs
crates/ruff_linter/src/rules/flake8_datetimez/rules/helpers.rs
crates/ruff_linter/src/rules/flake8_django/rules/helpers.rs
crates/ruff_linter/src/rules/flake8_executable/helpers.rs
crates/ruff_linter/src/rules/flake8_logging/rules/helpers.rs
crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs
crates/ruff_linter/src/rules/flake8_quotes/helpers.rs
crates/ruff_linter/src/rules/flake8_return/helpers.rs
crates/ruff_linter/src/rules/flake8_slots/rules/helpers.rs
crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs
crates/ruff_linter/src/rules/flynt/helpers.rs
crates/ruff_linter/src/rules/isort/helpers.rs
crates/ruff_linter/src/rules/numpy/helpers.rs
crates/ruff_linter/src/rules/pandas_vet/helpers.rs
crates/ruff_linter/src/rules/pep8_naming/helpers.rs
crates/ruff_linter/src/rules/perflint/helpers.rs
crates/ruff_linter/src/rules/pycodestyle/helpers.rs
crates/ruff_linter/src/rules/pydocstyle/helpers.rs
crates/ruff_linter/src/rules/pylint/helpers.rs
crates/ruff_linter/src/rules/pyupgrade/helpers.rs
crates/ruff_linter/src/rules/refurb/helpers.rs
crates/ruff_linter/src/rules/refurb/rules/helpers.rs
crates/ruff_linter/src/rules/ruff/rules/helpers.rs
crates/ruff_linter/src/rules/tryceratops/helpers.rs
crates/ruff_linter/src/text_helpers.rs
crates/ruff_python_ast/src/helpers.rs
crates/ruff_python_parser/src/parser/helpers.rs
```

---

_Comment by @dylwil3 on 2025-06-09 17:41_

>I did also notice the two helpers modules. Which way would you merge them? Looks like we have a pretty even mix of rules/LINTER/helpers.rs and rules/LINTER/rules/helpers.rs 😄 I'd probably lean toward LINTER/helpers.rs

yikes! yeah I agree - but maybe should be a separate PR

---

_@dylwil3 approved on 2025-06-09 17:42_

---

_Merged by @ntBre on 2025-06-09 20:07_

---

_Closed by @ntBre on 2025-06-09 20:07_

---

_Branch deleted on 2025-06-09 20:07_

---
