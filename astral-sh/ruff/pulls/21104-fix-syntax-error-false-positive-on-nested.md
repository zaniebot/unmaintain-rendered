```yaml
number: 21104
title: Fix syntax error false positive on nested alternative patterns
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/fix-syntax-error-alternations
created_at: 2025-10-27T21:03:35Z
updated_at: 2025-10-30T17:40:05Z
url: https://github.com/astral-sh/ruff/pull/21104
synced_at: 2026-01-12T15:57:16Z
```

# Fix syntax error false positive on nested alternative patterns

---

_@ntBre_

## Summary

Fixes #21101 by storing the child visitor's names in the parent visitor. This makes sure that `visitor.names` on line 1818 isn't empty after we visit a nested OR pattern.

## Test Plan

New inline test cases derived from the issue, [playground](https://play.ruff.rs/7b6439ac-ee8f-4593-9a3e-c2aa34a595d0)


---

_Label `bug` added by @ntBre on 2025-10-27 21:03_

---

_Comment by @github-actions[bot] on 2025-10-27 21:21_

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

_Marked ready for review by @ntBre on 2025-10-27 21:23_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-27 21:23_

---

_Review requested from @dhruvmanila by @ntBre on 2025-10-27 21:23_

---

_@amyreese approved on 2025-10-28 03:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1860 on 2025-10-28 18:19_

I don't have a good understanding of this part. So please disregard if not relevant or I'm just wrong ;)

Would it be better to return the union of all `visitor.names` here instead of just the last one in case we encountered a  `DifferentMatchPatternBindings` error? Or is there a risk that this introduces other false positives? If so, should we return the intersection of all names instead? 

(I suspect that either approach can lead to false positives depending on how the pattern are nested, so it might not be a case where there's no "better way" of doing this, it's just trade offs)

---

_@MichaReiser approved on 2025-10-28 18:19_

Thank you

---

_@ntBre reviewed on 2025-10-28 18:48_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1860 on 2025-10-28 18:48_

Ah I think that's a good question. (This part was not intuitive to me at all, I was working on a more complicated fix when I realized this solved the issue)

I guess this could come into play with cases like this:

```py
match 42:
    case [{1: x} | [x] | [y]] | [y]: ...

match 42:
    case [{1: x} | [x] | [y]] | [x]: ...
```

We currently emit two diagnostics for the first and only one for the second:

```
invalid-syntax: alternative patterns bind different names
  --> /tmp/s.py:11:10
   |
10 | match 42:
11 |     case [{1: x} | [x] | [y]] | [y]: ...
   |          ^^^^^^^^^^^^^^^^^^^^^^^^^^
12 |
13 | match 42:
   |

invalid-syntax: alternative patterns bind different names
  --> /tmp/s.py:11:11
   |
10 | match 42:
11 |     case [{1: x} | [x] | [y]] | [y]: ...
   |           ^^^^^^^^^^^^^^^^^^
12 |
13 | match 42:
   |

invalid-syntax: alternative patterns bind different names
  --> /tmp/s.py:14:11
   |
13 | match 42:
14 |     case [{1: x} | [x] | [y]] | [x]: ...
   |           ^^^^^^^^^^^^^^^^^^
   |
```

Hmm, we actually get the same answer here with the union, but we get two errors in both cases with the intersection.

Maybe that makes the intersection the best option for consistency's sake?

I think I'd consider these all true positives at least, but they may be a bit redundant. CPython only emits the innermost diagnostic, so we could also consider that.

This is also pretty hard to hit. Because of the `break` after emitting a `DifferentMatchPatternBindings` diagnostic, you need at least two inner patterns with the same name, followed by one with a different name.

---

_@ntBre reviewed on 2025-10-28 18:55_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1860 on 2025-10-28 18:55_

Oops, the intersection breaks the tests, I guess because `self.names` is initially empty. So I guess we should either stick with replacing or the union.

---

_@MichaReiser reviewed on 2025-10-29 15:41_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1860 on 2025-10-29 15:41_

Can't we just not intersect until we visited at least one pattern? Like use an `Option<FxHashSet>` and only intersect when it's some?

---

_@ntBre reviewed on 2025-10-29 16:44_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1860 on 2025-10-29 16:44_

I may be misunderstanding the suggestion, but I tried a patch like the one below, and it caused some other tests to fail (and without changing the new diagnostics from this PR). It's not just when `self.names` is initially empty as I said before, this also affects cases like:

```py
match x:
     case [] | [a]: ...
```

where one of the branches is genuinely empty.

I initially included the .snap.new files in this patch, but they were way too big. We lost the diagnostics in the cases above.

I'm leaning toward sticking with the simple existing code unless I'm just missing something.

<details><summary>Patch</summary>
<p>

```diff
diff --git a/crates/ruff_python_parser/src/semantic_errors.rs b/crates/ruff_python_parser/src/semantic_errors.rs
index f35029d4b9..3312c72544 100644
--- a/crates/ruff_python_parser/src/semantic_errors.rs
+++ b/crates/ruff_python_parser/src/semantic_errors.rs
@@ -11,7 +11,7 @@ use ruff_python_ast::{
     visitor::{Visitor, walk_expr},
 };
 use ruff_text_size::{Ranged, TextRange, TextSize};
-use rustc_hash::{FxBuildHasher, FxHashSet};
+use rustc_hash::{FxBuildHasher, FxHashMap, FxHashSet};
 use std::fmt::Display;
 
 #[derive(Debug, Default)]
@@ -117,7 +117,7 @@ impl SemanticSyntaxChecker {
                 Self::irrefutable_match_case(match_stmt, ctx);
                 for case in &match_stmt.cases {
                     let mut visitor = MatchPatternVisitor {
-                        names: FxHashSet::default(),
+                        names: None,
                         ctx,
                     };
                     visitor.visit_pattern(&case.pattern);
@@ -1671,7 +1671,7 @@ impl Visitor<'_> for ReboundComprehensionVisitor<'_> {
 }
 
 struct MatchPatternVisitor<'a, Ctx> {
-    names: FxHashSet<&'a ast::name::Name>,
+    names: Option<FxHashSet<&'a ast::name::Name>>,
     ctx: &'a Ctx,
 }
 
@@ -1810,15 +1810,19 @@ impl<'a, Ctx: SemanticSyntaxContext> MatchPatternVisitor<'a, Ctx> {
                 let mut previous_names: Option<FxHashSet<&ast::name::Name>> = None;
                 for pattern in patterns {
                     let mut visitor = Self {
-                        names: FxHashSet::default(),
+                        names: None,
                         ctx: self.ctx,
                     };
                     visitor.visit_pattern(pattern);
                     let Some(prev) = &previous_names else {
-                        previous_names = Some(visitor.names);
+                        previous_names = visitor.names;
                         continue;
                     };
-                    if prev.symmetric_difference(&visitor.names).next().is_some() {
+                    if visitor
+                        .names
+                        .as_ref()
+                        .is_some_and(|names| prev.symmetric_difference(&names).next().is_some())
+                    {
                         // test_err different_match_pattern_bindings
                         // match x:
                         //     case [a] | [b]: ...
@@ -1857,7 +1861,13 @@ impl<'a, Ctx: SemanticSyntaxContext> MatchPatternVisitor<'a, Ctx> {
                         );
                         break;
                     }
-                    self.names = visitor.names;
+                    if let Some(names) = &self.names
+                        && let Some(other) = visitor.names
+                    {
+                        self.names = Some(names.intersection(&other).cloned().collect());
+                    } else {
+                        self.names = visitor.names;
+                    }
                 }
             }
         }
@@ -1866,7 +1876,12 @@ impl<'a, Ctx: SemanticSyntaxContext> MatchPatternVisitor<'a, Ctx> {
     /// Add an identifier to the set of visited names in `self` and emit a [`SemanticSyntaxError`]
     /// if `ident` has already been seen.
     fn insert(&mut self, ident: &'a ast::Identifier) {
-        if !self.names.insert(&ident.id) {
+        if self.names.is_none() {
+            self.names = Some(FxHashSet::default());
+        }
+        let names = self.names.as_mut().unwrap();
+
+        if !names.insert(&ident.id) {
             SemanticSyntaxChecker::add_error(
                 self.ctx,
                 SemanticSyntaxErrorKind::MultipleCaseAssignment(ident.id.clone()),
```

</p>
</details> 

---

_@MichaReiser reviewed on 2025-10-29 17:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1860 on 2025-10-29 17:50_

That's not what I meant, I think? 

I'd created a `Option<FxHashSet>` right outside the `for pattern in patterns {` rather than globally in the visitor


But i also think that what we have is fine.

---

_@ntBre reviewed on 2025-10-29 17:55_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1860 on 2025-10-29 17:55_

We have one of those too:

https://github.com/astral-sh/ruff/blob/d38a5292d2a9c8509da6868046da0caa49ed5e46/crates/ruff_python_parser/src/semantic_errors.rs#L1809-L1811

We need an outer one in the visitor (or at least somewhere else) for recursive cases.

---

_@ntBre reviewed on 2025-10-30 17:39_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1860 on 2025-10-30 17:39_

I'll go ahead and land this for now then since it should resolve the bug!

---

_Merged by @ntBre on 2025-10-30 17:40_

---

_Closed by @ntBre on 2025-10-30 17:40_

---

_Branch deleted on 2025-10-30 17:40_

---
