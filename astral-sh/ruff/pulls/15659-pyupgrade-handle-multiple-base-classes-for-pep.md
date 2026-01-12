```yaml
number: 15659
title: "[`pyupgrade`] Handle multiple base classes for PEP 695 generics (`UP046`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/pep695-multi-base
created_at: 2025-01-21T23:17:52Z
updated_at: 2025-01-23T01:19:15Z
url: https://github.com/astral-sh/ruff/pull/15659
synced_at: 2026-01-12T15:55:52Z
```

# [`pyupgrade`] Handle multiple base classes for PEP 695 generics (`UP046`)

---

_@ntBre_

## Summary

Addresses the second follow up to #15565 in #15642. This was easier than expected by using this cool destructuring syntax I hadn't used before, and by assuming [PYI059](https://docs.astral.sh/ruff/rules/generic-not-last-base-class/) (`generic-not-last-base-class`).

## Test Plan

Using an existing test, plus two new tests combining multiple base classes and multiple generics. It looks like I deleted a relevant test, which I did, but I meant to rename this in #15565. It looks like instead I copied it and renamed the copy.

---

_Label `rule` added by @ntBre on 2025-01-21 23:19_

---

_Comment by @github-actions[bot] on 2025-01-21 23:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `preview` added by @AlexWaygood on 2025-01-22 18:04_

---

_Marked ready for review by @ntBre on 2025-01-22 18:38_

---

_@ntBre reviewed on 2025-01-22 18:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:208 on 2025-01-22 18:42_

I guess this could also be a single `Edit::replacement` if we concatenate `type_params.to_string()` and `base_classes` first. Would that be preferable?

---

_Review requested from @AlexWaygood by @ntBre on 2025-01-22 18:42_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046_0.py`:1 on 2025-01-22 18:46_

A couple more tests you could add:

```py
from typing import TypeVar, Generic

T = TypeVar("T")
S = TypeVar("S")

class A(Generic[T]): ...
class B(A[S], Generic[S]): ...
class C(A[S], Generic[S, T]): ...
class D(A[int], Generic[T]): ...
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:125 on 2025-01-22 18:50_

```suggestion
    // (generic-not-last-base-class). If it comes elsewhere, it results in a runtime error.
    // In stubs it's not *strictly* necessary for `Generic` to come last in the bases tuple,
    // but it would cause more complication for us to handle stubs specially,
    // and probably isn't worth the bother.
```

Could we also add a test for the case where `Generic` isn't last in the bases tuple? I'm particularly interested in whether we issue a diagnostic for classes where `Generic` is not last in the bases tuple. For classes like that, I think we should probably issue a diagnostic, but not a fix...?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:185 on 2025-01-22 18:50_

```suggestion
        if base_classes.is_empty() {
            // this means that `Generic[]` was the only base class;
            // we just replace the whole bases tuple with the type parameters
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:210 on 2025-01-22 18:53_

hmm... could this be simplified if we used this helper? https://github.com/astral-sh/ruff/blob/d981bf5f4bec7e78cf8ff6efd749ac9dfb4eb0a0/crates/ruff_linter/src/fix/edits.rs#L201-L257 ?

---

_@AlexWaygood approved on 2025-01-22 18:53_

Nice!

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046_0.py`:1 on 2025-01-22 19:03_

Oh interesting, I thought these would be invalid for inheriting from `Generic` multiple times, but they seem to work fine. They all move the `Generic`s to type parameters and keep the other base class. Is that right?

---

_@ntBre reviewed on 2025-01-22 19:03_

---

_@AlexWaygood reviewed on 2025-01-22 19:04_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046_0.py`:1 on 2025-01-22 19:04_

> They all move the `Generic`s to type parameters and keep the other base class. Is that right?

yup, that's the correct behaviour!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046_0.py`:1 on 2025-01-22 19:09_

> Oh interesting, I thought these would be invalid for inheriting from `Generic` multiple times, but they seem to work fine.

yeah, this is fine. It's a mechanism you can use to partially specialize a base class, or to add more type parameters on top of those that exist on a base class, or do both at once!

---

_@AlexWaygood reviewed on 2025-01-22 19:09_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:125 on 2025-01-22 19:23_

Ah, I was hoping we could add something like `tail_classes @ ..` to the end of the slice and magically handle `Generic` not being at the end, but you can only do `..` once in a slice pattern. We currently bail out too early to give a diagnostic here, but I'll try to fix that!

Would it be bad to offer a fix anyway? I think most of the effort will go into finding `Generic`; the fix logic should be mostly the same, if not identical.

---

_@ntBre reviewed on 2025-01-22 19:23_

---

_@AlexWaygood reviewed on 2025-01-22 19:26_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:125 on 2025-01-22 19:26_

> Would it be bad to offer a fix anyway?

I think so, because this is a stylistic rule, but if we offered a fix when `Generic` is in the middle of the bases list we'd be changing the behaviour of the program :/ we'd be turning broken code into code that would no longer raise an exception.

Now, that might be something that users would thank us for, of course 😄 but it still doesn't feel like something a _stylistic_ rule should be doing. If this rule's primary purpose was to complain about _possibly incorrect_ code, that would be a different question.

---

_@ntBre reviewed on 2025-01-22 20:12_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:210 on 2025-01-22 20:12_

Nice, very helpful!

---

_@ntBre reviewed on 2025-01-22 20:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:216 on 2025-01-22 20:22_

I just realized that neither this nor the destructuring version account for the possibility of multiple `Generic` arguments. They either grab the first (in this case) or last (when destructuring). Do we need to `filter_map` here and `collect` to check that the length is 1? Or is it safe to assume there's only one?

Before this PR there had to be a single `Generic` because there had to be a single base class total.

---

_@AlexWaygood reviewed on 2025-01-22 20:42_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:216 on 2025-01-22 20:42_

Oh, great catch! You're quite right -- this is another case where we should probably issue a diagnostic but not attempt a fix, since a class with multiple `Generic[]`s in the bases tuple will fail at runtime.

I think it should be possible to detect that without `collect`ing though -- collecting can be surprisingly expensive, as it involves allocating memory on the heap. Something like this?

```rs
let mut generic_bases_iter = <something involving filter_map>;

let Some(first_generic) = generic_bases_iter.next() else {
    return;
}

if generic_bases_iter.is_some() {
    // multiple `Generic[]`s in the bases tuple -- invalid!
    return;
}
```

---

_@ntBre reviewed on 2025-01-22 21:00_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:216 on 2025-01-22 21:00_

I think I had a second realization in the meantime :laughing: Since we're now `find`ing the first one, there will be more base classes after it, causing us to bail out with a diagnostic at the index check! So I don't think we need any more code, but I will add a test confirming this.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:227 on 2025-01-22 21:35_

A few nitpicks here, but none of these are major:
- The `as_ref()` call isn't necessary -- you can call `.iter()` on a `&Box<[Expr]>` expression just like a `&[Expr]` expression (`&Box<[Expr]>` dereferences to `&[Expr]`)
- I would just have the function take an argument of type `&SemanticModel` rather than `&mut Checker` -- we only need the semantic model here, and it's a much less "heavy" dependency for this helper function than the `Checker`
- I'd call the first parameter to the function `class_bases` or `bases_tuple` rather than `arguments` -- it has type `&Arguments`, but what the parameter _represents_ is the class's bases tuple

Put together, that implies the following changes:

<details>
<summary>Suggested patch</summary>

```diff
--- a/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs
+++ b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs
@@ -3,6 +3,7 @@ use ruff_macros::{derive_message_formats, ViolationMetadata};
 use ruff_python_ast::visitor::Visitor;
 use ruff_python_ast::ExprSubscript;
 use ruff_python_ast::{Arguments, StmtClassDef};
+use ruff_python_semantic::SemanticModel;
 use ruff_text_size::Ranged;
 
 use crate::checkers::ast::Checker;
@@ -124,7 +125,7 @@ pub(crate) fn non_pep695_generic_class(checker: &mut Checker, class_def: &StmtCl
     };
 
     let Some((generic_idx, generic_expr @ ExprSubscript { slice, range, .. })) =
-        find_generic(arguments, checker)
+        find_generic(arguments, checker.semantic())
     else {
         return;
     };
@@ -206,22 +207,16 @@ pub(crate) fn non_pep695_generic_class(checker: &mut Checker, class_def: &StmtCl
 }
 
 /// Search `arguments` for a `typing.Generic` base class. Returns the `Generic` expression (if any),
-/// along with its index in `arguments`.
+/// along with its index in the class's bases tuple.
 fn find_generic<'a>(
-    arguments: &'a Arguments,
-    checker: &mut Checker,
+    class_bases: &'a Arguments,
+    semantic: &SemanticModel,
 ) -> Option<(usize, &'a ExprSubscript)> {
-    arguments
-        .args
-        .as_ref()
-        .iter()
-        .enumerate()
-        .find_map(|(idx, expr)| {
-            expr.as_subscript_expr().and_then(|sub_expr| {
-                checker
-                    .semantic()
-                    .match_typing_expr(&sub_expr.value, "Generic")
-                    .then_some((idx, sub_expr))
-            })
+    class_bases.args.iter().enumerate().find_map(|(idx, expr)| {
+        expr.as_subscript_expr().and_then(|sub_expr| {
+            semantic
+                .match_typing_expr(&sub_expr.value, "Generic")
+                .then_some((idx, sub_expr))
         })
+    })
 }
```

</details>

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:202 on 2025-01-22 21:39_

We have a `try_set_fix()` method that's handy for cases like this!

```suggestion
        diagnostic.try_set_fix(|| {
            let removal_edit = remove_argument(
                generic_expr,
                arguments,
                Parentheses::Remove,
                checker.source(),
            )?;
            Ok(Fix::unsafe_edits(
                Edit::insertion(type_params.to_string(), name.end()),
                [removal_edit],
            ))
        });
```

The advantage is that if you run Ruff with enough `-v` flags, it prints a useful error message to the terminal explaining why it was unable to provide a fix for the specific case where `remove_argument()` returned an error

---

_@AlexWaygood approved on 2025-01-22 21:39_

---

_@ntBre reviewed on 2025-01-22 22:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:227 on 2025-01-22 22:55_

Ah thanks, this looks a lot better!

---

_@ntBre reviewed on 2025-01-22 23:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:202 on 2025-01-22 23:02_

That is very handy! I was wondering how `remove_argument` could fail at all, so I'll be glad to see those messages if it does!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs`:5 on 2025-01-22 23:04_

```suggestion
use ruff_python_ast::{Arguments, ExprSubscript, StmtClassDef};
```

---

_@AlexWaygood approved on 2025-01-22 23:05_

🚢 it!

---

_Merged by @ntBre on 2025-01-23 01:19_

---

_Closed by @ntBre on 2025-01-23 01:19_

---

_Branch deleted on 2025-01-23 01:19_

---
