```yaml
number: 9614
title: "Add `parenthesized` flag to `ExprTuple` and `ExprGenerator`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: tuple_is_parenthesized
created_at: 2024-01-22T13:14:03Z
updated_at: 2024-02-26T15:46:22Z
url: https://github.com/astral-sh/ruff/pull/9614
synced_at: 2026-01-12T15:55:29Z
```

# Add `parenthesized` flag to `ExprTuple` and `ExprGenerator`

---

_@MichaReiser_

## Summary

This PR adds an `parenthesized` prop to `ExprTuple` and `ExprGenerator` that expose if the tuple/genrator is parenthesized in the source. 

The information is useful during linting and formatting, and dedecting if a tuple is parenthesized is somewhat involved. 

Should this information be present in the AST: IMO, properties that are needed more than once are worth tracking in the AST, even if they don't have any semantical meaning. 
It also simplifies implementing the new parser where it otherwise would need to be necessary to track the information separately. 

## Test Plan

I added new tests and reviewed the snapshot changes


---

_Converted to draft by @MichaReiser on 2024-01-22 13:14_

---

_Label `internal` added by @MichaReiser on 2024-01-22 13:14_

---

_Label `parser` added by @MichaReiser on 2024-01-22 13:14_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:75 on 2024-01-22 13:16_

What's the benefit of doing this over

```suggestion
                    ..,
```

?

---

_@AlexWaygood reviewed on 2024-01-22 13:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:75 on 2024-01-22 13:43_

It seems the author intended to handle each property explicitly. I didn't want to change that decision. 

---

_@MichaReiser reviewed on 2024-01-22 13:43_

---

_Comment by @github-actions[bot] on 2024-01-22 14:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Comment by @charliermarsh on 2024-01-22 14:19_

If we track this for tuples, should we also track it for generators, where it's also optional?

---

_Comment by @charliermarsh on 2024-01-22 14:19_

I def support this. I think I tried to pitch it at one point but there wasn't enough interest :joy:

---

_Renamed from "Add `is_parenthesized` flag to `ExprTuple`" to "Add `is_parenthesized` flag to `ExprTuple` and `ExprGenerator`" by @MichaReiser on 2024-02-26 13:30_

---

_Renamed from "Add `is_parenthesized` flag to `ExprTuple` and `ExprGenerator`" to "Add `parenthesized` flag to `ExprTuple` and `ExprGenerator`" by @MichaReiser on 2024-02-26 13:31_

---

_Marked ready for review by @MichaReiser on 2024-02-26 13:38_

---

_@charliermarsh approved on 2024-02-26 15:08_

---

_@AlexWaygood reviewed on 2024-02-26 15:10_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:156 on 2024-02-26 15:10_

The code for RUF022 and RUF023 can be further simplified -- the only reason we were previously keeping a reference to the `ExprTuple` node around here was so that we could lazily query later whether the original tuple was parenthesized or not. With your refactor here, we can just store that directly by doing something like this:

```diff
--- a/crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs
@@ -135,13 +135,13 @@ impl InferredMemberType {
 /// We keep the original AST node around for the
 /// Tuple variant so that this can be queried later.
 #[derive(Debug)]
-pub(super) enum SequenceKind<'a> {
+pub(super) enum SequenceKind {
     List,
     Set,
-    Tuple(&'a ast::ExprTuple),
+    Tuple { parenthesized: bool },
 }
 
-impl SequenceKind<'_> {
+impl SequenceKind {
     // N.B. We only need the source code for the Tuple variant here,
     // but if you already have a `Locator` instance handy,
     // getting the source code is very cheap.
@@ -149,11 +149,8 @@ impl SequenceKind<'_> {
         match self {
             Self::List => ("[", "]"),
             Self::Set => ("{", "}"),
-            Self::Tuple(ast::ExprTuple {
-                parenthesized: is_parenthesized,
-                ..
-            }) => {
-                if *is_parenthesized {
+            Self::Tuple { parenthesized } => {
+                if *parenthesized {
                     ("(", ")")
                 } else {
                     ("", "")
@@ -166,7 +163,7 @@ impl SequenceKind<'_> {
         match self {
             Self::List => TokenKind::Lsqb,
             Self::Set => TokenKind::Lbrace,
-            Self::Tuple(_) => TokenKind::Lpar,
+            Self::Tuple { .. } => TokenKind::Lpar,
         }
     }
 
@@ -174,7 +171,7 @@ impl SequenceKind<'_> {
         match self {
             Self::List => TokenKind::Rsqb,
             Self::Set => TokenKind::Rbrace,
-            Self::Tuple(_) => TokenKind::Rpar,
+            Self::Tuple { .. } => TokenKind::Rpar,
         }
     }
 }
diff --git a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
index 6df5646f6..d1a0ce79a 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs
@@ -153,7 +153,7 @@ fn sort_dunder_all(checker: &mut Checker, target: &ast::Expr, node: &ast::Expr)
     let (elts, range, kind) = match node {
         ast::Expr::List(ast::ExprList { elts, range, .. }) => (elts, *range, SequenceKind::List),
         ast::Expr::Tuple(tuple_node @ ast::ExprTuple { elts, range, .. }) => {
-            (elts, *range, SequenceKind::Tuple(tuple_node))
+            (elts, *range, SequenceKind::Tuple{ parenthesized: tuple_node.parenthesized })
         }
         _ => return,
     };
diff --git a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs
index 83248ac9d..64c34879b 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs
@@ -157,7 +157,9 @@ impl<'a> StringLiteralDisplay<'a> {
                 }
             }
             ast::Expr::Tuple(tuple_node @ ast::ExprTuple { elts, range, .. }) => {
-                let display_kind = DisplayKind::Sequence(SequenceKind::Tuple(tuple_node));
+                let display_kind = DisplayKind::Sequence(SequenceKind::Tuple {
+                    parenthesized: tuple_node.parenthesized,
+                });
                 Self {
                     elts: Cow::Borrowed(elts),
                     range: *range,
@@ -242,7 +244,7 @@ impl<'a> StringLiteralDisplay<'a> {
 /// Python provides for builtin containers.
 #[derive(Debug)]
 enum DisplayKind<'a> {
-    Sequence(SequenceKind<'a>),
+    Sequence(SequenceKind),
     Dict { values: &'a [ast::Expr] },
```

---

_@MichaReiser reviewed on 2024-02-26 15:28_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:156 on 2024-02-26 15:28_

Ahh nice catch!

---

_Merged by @MichaReiser on 2024-02-26 15:35_

---

_Closed by @MichaReiser on 2024-02-26 15:35_

---

_Branch deleted on 2024-02-26 15:35_

---
