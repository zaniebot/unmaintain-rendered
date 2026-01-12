```yaml
number: 17214
title: "[red-knot] Add inlay type hints"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: inlay-hints
created_at: 2025-04-04T21:49:07Z
updated_at: 2025-04-19T13:58:36Z
url: https://github.com/astral-sh/ruff/pull/17214
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Add inlay type hints

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #17205

## Test Plan

Add tests to red_knot_ide/src/inlay_hints.rs

https://github.com/user-attachments/assets/f3e654c5-d95b-4de3-91ae-602843c160d1

https://github.com/user-attachments/assets/fcc1861a-aa34-49c3-ad6f-fb375dc546d8





---

_Comment by @github-actions[bot] on 2025-04-04 21:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MatthewMckee4 on 2025-04-04 21:53_

Currently this adds type hints to almost everything, need to work on that

---

_Comment by @MatthewMckee4 on 2025-04-04 21:59_

Also need to make sure we dont add type hints when there is already one there 

---

_Label `red-knot` added by @AlexWaygood on 2025-04-04 22:24_

---

_Comment by @MatthewMckee4 on 2025-04-05 15:10_

@MichaReiser suggested the following

I think I'd take an approach closer to rust analyzer:

* Write a custom `SourceOrderVisitor` that traverses the AST
* `enter_node` returns `TraverseSignal::Skip` if the node isn't in the queried range
* Compute the hints in `visit_*` methods but only for the nodes you want by using `node.inferred_type` (because we never want to show declared types?)

https://github.com/rust-lang/rust-analyzer/blob/15d87419f1a123d8f888d608129c3ce3ff8f13d4/crates/ide/src/inlay_hints.rs#L224-L283

---

_Marked ready for review by @MatthewMckee4 on 2025-04-05 15:10_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-05 15:10_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-05 15:10_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-05 15:10_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-05 15:10_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-04-05 15:10_

---

_Comment by @MatthewMckee4 on 2025-04-05 15:21_

Im not sure how to work with settings to be able to enable/disable this

---

_Comment by @MichaReiser on 2025-04-05 15:31_

Did you push your latest changes? The current approach doesn't use a custom visitor. Could you add a test plan so that I get a sense for how the feature works?

---

_Comment by @MatthewMckee4 on 2025-04-05 15:37_

Ive not implemented a custom visitor yet, ill get on that. `red_knot_python_semantic/src/visitor.rs`?

---

_Converted to draft by @MatthewMckee4 on 2025-04-05 15:46_

---

_Comment by @MatthewMckee4 on 2025-04-05 22:44_

![image](https://github.com/user-attachments/assets/7c445a00-8cfc-42d9-a3a5-d49267156de6)
![image](https://github.com/user-attachments/assets/f22a7b03-6011-4f71-9373-ccb982911fda)

Does the first option look good?

---

_Comment by @InSyncWithFoo on 2025-04-05 22:59_

@MatthewMckee4 The second is more pleasing to the eyes, but I guess the hints won't be as nice for something like `(a1, *a2) = a`.

---

_Comment by @MatthewMckee4 on 2025-04-05 23:07_

@InSyncWithFoo Yeah the hints dont change from the second image with `(a1, *a2) = a`

---

_Comment by @MatthewMckee4 on 2025-04-05 23:28_

Personally
![image](https://github.com/user-attachments/assets/72e19518-fdcf-400a-b9ac-2e5e6a4404c5)
This does not look as nice or useful as
![image](https://github.com/user-attachments/assets/bc712f0e-bd2a-4c3d-8db7-ea91864a47e8)


---

_Comment by @MatthewMckee4 on 2025-04-05 23:42_

I think theres lots more we can add to (or remove from) this:
```py
# This is not very useful
a: Literal["abc"] = "abc"
b: Literal[1] = 1
... more `Literal` values

c: int | float | complex = 1 + 2j
d: None = None
e: Literal[True] = True
```
And probably some more

edit: maybe im wrong here (i guess the point of using this is to provide these insights).

Any input is appreciated

---

_Marked ready for review by @MatthewMckee4 on 2025-04-05 23:43_

---

_Converted to draft by @MatthewMckee4 on 2025-04-06 02:53_

---

_Comment by @MichaReiser on 2025-04-06 07:46_

I prefer the first. Maybe because it's also what r-a does. TypeScript doesn't show inlay hints for destructuring (unpacking). Most LSPs have very fine grained settings for in which positions inlay hints should be shown. 

For now, I think we should just pick one and we can iterate based on user feedback

---

_Comment by @dhruvmanila on 2025-04-06 14:22_

> Personally
> 
> ![image](https://github.com/user-attachments/assets/72e19518-fdcf-400a-b9ac-2e5e6a4404c5)
> 
> This does not look as nice or useful as
> 
> ![image](https://github.com/user-attachments/assets/bc712f0e-bd2a-4c3d-8db7-ea91864a47e8)
> 
> 

I think I'd prefer the latter as for the first one could get confused that the type hint belongs to the last variable if the tuple doesn't contain parentheses. This might also be a bit subjective.

Edit: I think that's what you meant now that I read it again?

---

_Marked ready for review by @MatthewMckee4 on 2025-04-06 14:36_

---

_Comment by @AlexWaygood on 2025-04-06 14:49_

I think I'm making a similar point to @MatthewMckee4 in https://github.com/astral-sh/ruff/pull/17214#issuecomment-2781134667 here... I think we should maybe think about the _use cases_ for inlay type hints.

I find rust-analyzer's inlay type hints useful in two situations:
1. Telling me what type the compiler infers for "intermediate steps" in long/complex call chains, e.g.

   <details>
   <summary>Screenshot</summary>

   ![image](https://github.com/user-attachments/assets/276025ba-f4c8-4a66-8206-44bfcd70f058)

   </details>
2. Providing me an easy way to add type annotations to my code if I want to be explicit and/or to be certain that the compiler is inferring the type I want: I can just double-click on the inlay.

I think these two things are sort-of in conflict with each other a little bit for us? And possibly neither motivation is quite as strong as it is for rust-analyzer?
1. Providing a way of telling me what each step in a long call chain is inferred as would still be useful in some situations. However, long call chains aren't nearly as common in Python as they are in Rust. (You find them a lot in code that uses some specific libraries like pandas, but not much outside of those use cases.) I think we're also less likely to infer complex generics as the Rust compiler is (though we do have intersections/unions, which may get complex in some situations).
2. When it comes to providing an easy way to add type annotations to your code: we infer extremely precise types for local variables -- but you'd almost never want to use those extremely precise types in explicit type annotations. `x: Literal[1] = 1` is an annotation you'd never see in idiomatic Python code: it would either be `x: Final = 1` (indicating that you should never assign to `x` again after the variable's first intialisation) or some broader type like `x: int = `, `x: typing.SupportsIndex = 1` or `x: int | None = 1` (which would allow you to assign a different value to `x` after its first initialisation). So I wouldn't find the `Literal[1]` inlay on something like `x = 1` very useful

That said, if these are going to be opt-in only then I'm fine with landing an initial version of this and iterating on it! We can see whether people find it useful or not. I might also be missing some use cases for inlays?

---

_Comment by @MatthewMckee4 on 2025-04-06 14:57_

I think one other useful thing that @InSyncWithFoo mentioned is inlay hints for inferred function return types, but im not sure we are at the stage right now where we can do that

---

_Comment by @carljm on 2025-04-06 15:25_

Should we provide inlays that are not valid Python syntax? Neither `a, b: tuple[...] = ...` nor `a: ..., b: ... = ...` is valid Python. Annotated assignments can't be unpacked, regular assignments can't take annotations. 

---

_Comment by @MatthewMckee4 on 2025-04-06 15:34_

That's a good point, but rust-analyzer also provides inlay hints for invalid annotations, for the unpacking here
![image](https://github.com/user-attachments/assets/2b6a1782-163d-4464-9c66-8acb9834e8bc)
And because of that you cant double click on it (to insert the type)

---

_Comment by @MichaReiser on 2025-04-06 17:12_

> I find rust-analyzer's inlay type hints useful in two situations:

I don't think this PR implements 2.

From the LSP specification

> /**
	 * Optional text edits that are performed when accepting this inlay hint.
	 *
	 * *Note* that edits are expected to change the document so that the inlay
	 * hint (or its nearest variant) is now part of the document and the inlay
	 * hint itself is now obsolete.
	 *
	 * Depending on the client capability `inlayHint.resolveSupport` clients
	 * might resolve this property late using the resolve request.
	 */
	textEdits?: [TextEdit](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textEdit)[];

That means, we could generalize the type. Although I'm not quiet sure what the rules would be ;) But I don't think this should block this PR

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:42 on 2025-04-07 07:33_

Nit: We tend to omit the `get` prefix. 
```suggestion
pub fn inlay_hints(db: &dyn Db, file: File) -> Vec<RangedValue<InlayHintContent<'_>>> {
```

---

_@MichaReiser reviewed on 2025-04-07 07:33_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/requests/inlay_hints.rs`:48 on 2025-04-07 07:34_

The `InlayHintRequestHandler` provide a `range` to avoid computing the inlay hints for the entire document. We should respect that parameter

---

_@MichaReiser reviewed on 2025-04-07 07:34_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:52 on 2025-04-07 07:37_


```suggestion
    visitor.hints
}
```

---

_@MichaReiser reviewed on 2025-04-07 07:37_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:64 on 2025-04-07 07:38_

I suggest not wrapping `InlayHintContent` in a `RangedValue` because, unlike goto type definition, the hints must always belong to `file` (the LSP doesn't allow you to return hints that belong to another file). 

That's why I simply storing a `TextRange` for each hint should be simpler.

Looking at the usage in the LSP, it seems all we need is a `TextSize`, the position where the inlay hint should be shown.

---

_@MichaReiser reviewed on 2025-04-07 07:39_

---

_@MatthewMckee4 reviewed on 2025-04-07 07:43_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_ide/src/inlay_hints.rs`:64 on 2025-04-07 07:43_

For the lsp we need line number and column number, inside the wasm there's an easy way to do this, with 'Range', but outside it's not so easy, what do you think we should do 

---

_@MichaReiser reviewed on 2025-04-07 07:49_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:64 on 2025-04-07 07:49_

Can you explain what isn't easy? We do similar conversions in goto type definition and on hover.

---

_@MatthewMckee4 reviewed on 2025-04-07 07:50_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_ide/src/inlay_hints.rs`:64 on 2025-04-07 07:50_

I guess it just means I need to copy code from the wasm, to convert text range to line and column number 

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/requests/inlay_hints.rs`:65 on 2025-04-07 07:52_

The conversion is a bit more involved because the column depends on the encoding used by the client. We already have helpers for this conversion but they are currently not exposed. Let's introduce `TextSizeExt` trait and add a `to_position` method

```patch
Subject: [PATCH] [red-knot] Add 'Format document' to playground
---
Index: crates/red_knot_server/src/document/range.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_server/src/document/range.rs b/crates/red_knot_server/src/document/range.rs
--- a/crates/red_knot_server/src/document/range.rs	(revision c1c5434abe524b4e1ecb6e80a82f668cfcf27dcb)
+++ b/crates/red_knot_server/src/document/range.rs	(date 1744012168728)
@@ -27,6 +27,29 @@
     fn to_text_size(&self, text: &str, index: &LineIndex, encoding: PositionEncoding) -> TextSize;
 }
 
+pub(crate) trait TextSizeExt {
+    fn to_position(
+        self,
+        text: &str,
+        index: &LineIndex,
+        encoding: PositionEncoding,
+    ) -> lsp_types::Position
+    where
+        Self: Sized;
+}
+
+impl TextSizeExt for TextSize {
+    fn to_position(
+        self,
+        text: &str,
+        index: &LineIndex,
+        encoding: PositionEncoding,
+    ) -> lsp_types::Position {
+        let source_location = offset_to_source_location(self, text, index, encoding);
+        source_location_to_position(&source_location)
+    }
+}
+
 pub(crate) trait ToRangeExt {
     fn to_lsp_range(
         &self,
@@ -104,18 +127,8 @@
         encoding: PositionEncoding,
     ) -> types::Range {
         types::Range {
-            start: source_location_to_position(&offset_to_source_location(
-                self.start(),
-                text,
-                index,
-                encoding,
-            )),
-            end: source_location_to_position(&offset_to_source_location(
-                self.end(),
-                text,
-                index,
-                encoding,
-            )),
+            start: self.start().to_position(text, index, encoding),
+            end: self.end().to_position(text, index, encoding),
         }
     }
 
Index: crates/red_knot_server/src/server/api/requests/inlay_hints.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_server/src/server/api/requests/inlay_hints.rs b/crates/red_knot_server/src/server/api/requests/inlay_hints.rs
--- a/crates/red_knot_server/src/server/api/requests/inlay_hints.rs	(revision c1c5434abe524b4e1ecb6e80a82f668cfcf27dcb)
+++ b/crates/red_knot_server/src/server/api/requests/inlay_hints.rs	(date 1744012202304)
@@ -1,5 +1,6 @@
 use std::borrow::Cow;
 
+use crate::document::{FileRangeExt, TextSizeExt, ToRangeExt};
 use crate::server::api::traits::{BackgroundDocumentRequestHandler, RequestHandler};
 use crate::server::client::Notifier;
 use crate::DocumentSnapshot;
@@ -52,24 +53,18 @@
 
         let inlay_hints = inlay_hints
             .into_iter()
-            .map(|hint| {
-                let end = index.source_location(hint.range.range().end(), &source);
-
-                lsp_types::InlayHint {
-                    position: lsp_types::Position {
-                        line: u32::try_from(end.row.to_zero_indexed())
-                            .expect("row usize fits in u32"),
-                        character: u32::try_from(end.column.to_zero_indexed())
-                            .expect("character usize fits in u32"),
-                    },
-                    label: lsp_types::InlayHintLabel::String(hint.display(&db).to_string()),
-                    kind: None,
-                    tooltip: None,
-                    padding_left: None,
-                    padding_right: None,
-                    data: None,
-                    text_edits: None,
-                }
+            .map(|hint| lsp_types::InlayHint {
+                position: hint
+                    .range
+                    .end()
+                    .to_position(&source, &index, snapshot.encoding()),
+                label: lsp_types::InlayHintLabel::String(hint.display(&db).to_string()),
+                kind: None,
+                tooltip: None,
+                padding_left: None,
+                padding_right: None,
+                data: None,
+                text_edits: None,
             })
             .collect();
 
Index: crates/red_knot_server/src/document.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_server/src/document.rs b/crates/red_knot_server/src/document.rs
--- a/crates/red_knot_server/src/document.rs	(revision c1c5434abe524b4e1ecb6e80a82f668cfcf27dcb)
+++ b/crates/red_knot_server/src/document.rs	(date 1744012052127)
@@ -8,7 +8,7 @@
 pub(crate) use location::ToLink;
 use lsp_types::{PositionEncodingKind, Url};
 pub use notebook::NotebookDocument;
-pub(crate) use range::{FileRangeExt, PositionExt, RangeExt, ToRangeExt};
+pub(crate) use range::{FileRangeExt, PositionExt, RangeExt, TextSizeExt, ToRangeExt};
 pub(crate) use text_document::DocumentVersion;
 pub use text_document::TextDocument;
 

```

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/requests/inlay_hints.rs`:42 on 2025-04-07 07:54_

We don't want to store editor options in the project's metadata because these settings aren't project specific but user specific. Different users working on the same project will want to use different settings or even editors. 
Let's always enable inlay hints for now.

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/requests/inlay_hints.rs`:71 on 2025-04-07 07:55_

Nit: Not for this PR but we want to use the `InlayHintResolveRequest` to lazily resolve the tooltip and text edits. It may be worth adding a comment, see https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#inlayHint_resolve

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api.rs`:41 on 2025-04-07 07:56_

I realised when reviewing your PR that we used the incorrect scheduling for almost all our request handlers. I already updated the other request handlers in a separate PR but we also need to change the priority for inlay hints: Inlay hints aren't latency sensitive (they aren't in the hot path of blocking the user from typing). 



```suggestion
            req, BackgroundSchedule::Worker
```

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:279 on 2025-04-07 07:58_

I think we also want to pass the range here to avoid computing inlay hints for the entire document

---

_Review comment by @MichaReiser on `playground/knot/src/Editor/Editor.tsx`:203 on 2025-04-07 08:01_

Annotations like these in a lambda are uncommon. It's more common to annotate the variable `inlayHints`. But my preference here is to rely on TypeScript's inference
```suggestion
        hint => {
          return {
```

---

_Review comment by @MichaReiser on `playground/knot/src/Editor/Editor.tsx`:210 on 2025-04-07 08:02_

You can wrap the returned object in `(` to avoid the explicit return
```suggestion
        }) => ({
        label: hint.markdown,
        position: {
            lineNumber: hint.position.line,
            column: hint.position.column,
        },
        })
```

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:64 on 2025-04-07 08:05_

I don't think that should be necessary. We already have plenty of helpers in https://github.com/astral-sh/ruff/blob/a4ba10ff0ae89d331422d50c87c0ac0691ee0161/crates/red_knot_server/src/document/range.rs#L14

Unless you talk about something else (I'm not sure what you find hard)

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/requests/inlay_hints.rs`:66 on 2025-04-07 08:07_

We should set `kind` to `InlayHintKind::Type`

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:93 on 2025-04-07 08:08_

Hmm, this is a bit unfortunate because it re-implements unpacking but I think it's fine for now.

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:96 on 2025-04-07 08:08_


```suggestion
                for target in &assign.targets {
                    match target {
                        Expr::Tuple(tuple) => {
                            for element in &tuple.elts {
                                let element_ty = element.inferred_type(&self.model);
                                self.add_hint(element.range(), element_ty);
                            }
                        }
                        _ => {
                            let ty = assign.value.inferred_type(&self.model);
                            self.add_hint(target.range(), ty);
                        }
```

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:152 on 2025-04-07 08:09_

Could we use `Diagnostic` rendering with inline `insta` snapshots here similar to the goto type definition and on-hover tests? Tests with text ranges are always very hard to read and the diagnostic rendering can help because it visualizes the ranges (much easier to spot errors)

---

_@MichaReiser requested changes on 2025-04-07 08:10_

This is great. We should respect the range provided by the editor and I don't think the options approach is what we want, let's remove that for now and always enable inlay hints. 

Could you update the test plan with a recording of the playground and LSP? 

---

_@MatthewMckee4 reviewed on 2025-04-07 08:21_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_server/src/server/api/requests/inlay_hints.rs`:42 on 2025-04-07 08:21_

Okay, ill just completely remove the options

---

_Comment by @MatthewMckee4 on 2025-04-07 08:51_

I might not be able to get to this for the rest of the day, if you want to have a go at updating the tests or the range then go for it!

---

_Comment by @MichaReiser on 2025-04-07 09:19_

> I might not be able to get to this for the rest of the day, if you want to have a go at updating the tests or the range then go for it!

There's no hurry and the chances for merge conflicts are very low. Take your time. 

---

_Renamed from "[red-knot] Add (optional) inlay type hints" to "[red-knot] Add inlay type hints" by @MatthewMckee4 on 2025-04-07 12:58_

---

_@dhruvmanila reviewed on 2025-04-08 14:47_

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/inlay_hints.rs`:93 on 2025-04-08 14:47_

I think this unpacking will only work for the top-level tuple and not for nested unpacking like `(a, (b, c)) = (1, (2, 3))`.

---

_@dhruvmanila reviewed on 2025-04-08 14:51_

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/inlay_hints.rs`:93 on 2025-04-08 14:51_

You might need to recursively call `visit_expr` and add the hint at `Expr::Name` node which should solve this.

---

_Review comment by @MatthewMckee4 on `crates/red_knot_ide/src/inlay_hints.rs`:93 on 2025-04-08 14:53_

thats true, ill look into this more

---

_@MatthewMckee4 reviewed on 2025-04-08 14:53_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:98 on 2025-04-09 06:52_

I wonder if we could use the same approach as in `SemanticIndexBuilder` where we have a state variable on `InlayHintVisitor` indicating if it is currently visiting an assignment. 

Then, inside `visit_expr`, add an inlay hint for every `ExprName` in a `Store` context. 

This has the benefit that the visitor doesn't traverse the assignment targets multiple times.

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:79 on 2025-04-09 06:53_

NIt: `add_assign_statement_hint` or should we instead rename the `InlayHintContent` variant` to `Type` and `ReturnType` and rename this method `add_type_hint` and add another `add_return_type_hint`. I think I'd like those variant names better.

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:104 on 2025-04-09 06:53_

You don't need this. `enter_node` gets called by `walk_stmt`
```suggestion
```

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:197 on 2025-04-09 06:55_

Oh, I like how you render the "hints" inline! 

I'm happy to go with this for now. An alternative would be to use the `Diagnostic` again and render an `annotation` at `hint.range.end` where the primary message is the hinted type. 



---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:14 on 2025-04-09 06:56_

Could this just be a `TextSize`? It seems all usages use `range.end` always

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/inlay_hints.rs`:207 on 2025-04-09 06:59_

Nit: You could use `<START>` and `<END>` end markers to indicate the range in the text file and default to the entire range if both the `SOURCE` and `END` markers are missing

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/requests/inlay_hints.rs`:49 on 2025-04-09 07:01_


```suggestion
        let range = params
            .range
            .to_text_range(&source, &index, snapshot.encoding());

```

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:379 on 2025-04-09 07:02_

I don't think this is needed, unless you want the JS client to create `Range` instances. 

For the rust code, using `Range { start, end }` should do it

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:428 on 2025-04-09 07:04_

We should make this return a `Result` instead of panicking if the line number is one indexed. 

We can then also use it on line 261, and 219

---

_@MichaReiser reviewed on 2025-04-09 07:05_

This looks good. I've a few smaller nit comments that would be great to address beforehand.

Please, also updated your PR summary with a test plan for both playground and the vs code extension (record a short video, similar to what I did with on hover)

---

_@MatthewMckee4 reviewed on 2025-04-09 09:06_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_ide/src/inlay_hints.rs`:104 on 2025-04-09 09:06_

True, but walk_stmt is not called as we override visit_stmt. The last test fails if I remove this line

---

_Review comment by @MatthewMckee4 on `crates/red_knot_ide/src/inlay_hints.rs`:98 on 2025-04-09 09:19_

I believe we only traverse (and call inferred_type) on assignments like
```py
a = b = 1
```

> Then, inside visit_expr, add an inlay hint for every ExprName in a Store context.

How would we get the type in visit_expr?

---

_@MatthewMckee4 reviewed on 2025-04-09 09:19_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_wasm/src/lib.rs`:379 on 2025-04-09 09:29_

i dont think it was working as it was doing some sort of instance check on range

---

_@MatthewMckee4 reviewed on 2025-04-09 09:29_

---

_Review request for @carljm removed by @carljm on 2025-04-09 15:00_

---

_@dhruvmanila reviewed on 2025-04-09 16:10_

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/inlay_hints.rs`:197 on 2025-04-09 16:10_

I think I've a slight preference to using diagnostics but that doesn't need to block this PR and can be done as a follow-up. My main reasoning is that other developers would need to look at both the test and the implementation to understand that the test infra adds the brackets (`[]`) and the implementation adds the `:` and `->`. The diagnostics has the benefit to say that "here is where this hint will be added".

---

_Review request for @sharkdp removed by @sharkdp on 2025-04-09 17:02_

---

_@MichaReiser approved on 2025-04-10 09:19_

---

_Merged by @MichaReiser on 2025-04-10 09:21_

---

_Closed by @MichaReiser on 2025-04-10 09:21_

---

_Branch deleted on 2025-04-19 13:58_

---
