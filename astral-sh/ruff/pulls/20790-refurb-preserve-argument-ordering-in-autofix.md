```yaml
number: 20790
title: "[`refurb`] Preserve argument ordering in autofix (`FURB103`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: furb103-fix-fix
created_at: 2025-10-09T15:47:38Z
updated_at: 2025-10-31T15:35:47Z
url: https://github.com/astral-sh/ruff/pull/20790
synced_at: 2026-01-10T16:59:49Z
```

# [`refurb`] Preserve argument ordering in autofix (`FURB103`)

---

_Pull request opened by @chirizxc on 2025-10-09 15:47_

Fixes https://github.com/astral-sh/ruff/issues/20785

---

_Comment by @github-actions[bot] on 2025-10-09 16:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+9 -9 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L75'>examples/models/buttons.py:75:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(doc, title="Button widgets"))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L75'>examples/models/buttons.py:75:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Button widgets", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L104'>examples/models/calendars.py:104:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(doc, title="Calendar 2014"))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L104'>examples/models/calendars.py:104:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Calendar 2014", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L107'>examples/models/daylight.py:107:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(doc, title="Daylight Plot"))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L107'>examples/models/daylight.py:107:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Daylight Plot", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L110'>examples/models/gauges.py:110:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(doc, title="Gauges"))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L110'>examples/models/gauges.py:110:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Gauges", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L115'>examples/models/glyphs.py:115:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(doc, title="Glyphs"))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L115'>examples/models/glyphs.py:115:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Glyphs", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/sliders.py#L74'>examples/models/sliders.py:74:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(doc, title="sliders"))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/sliders.py#L74'>examples/models/sliders.py:74:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="sliders", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/twin_axis.py#L57'>examples/models/twin_axis.py:57:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(doc, title="Twin Axis Plot"))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/twin_axis.py#L57'>examples/models/twin_axis.py:57:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Twin Axis Plot", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/widgets.py#L299'>examples/models/widgets.py:299:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(doc, title="Widgets"))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/widgets.py#L299'>examples/models/widgets.py:299:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Widgets", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/file_html.py#L122'>examples/output/apis/file_html.py:122:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(doc, title=plot.title.text))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/file_html.py#L122'>examples/output/apis/file_html.py:122:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title=plot.title.text, doc))`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB103 | 18 | 9 | 9 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `bug` added by @ntBre on 2025-10-09 16:36_

---

_Label `fixes` added by @ntBre on 2025-10-09 16:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:176 on 2025-10-09 17:26_

If I'm following this correctly, the bug is that we're relocating the argument to a function call within the `f.write` call when we're supposed to be relocating the argument to `f.write` itself. Is that accurate?

If so, it seems like the root problem might actually be the `arg` we're passing here and extracting in `match_write_call`. It doesn't seem like we should be rewriting the arguments to `json.dumps` at all.

---

_@ntBre reviewed on 2025-10-09 17:27_

Wow, thank you! I should have noticed this in the ecosystem check from the previous PR. Thanks for following up!

This fix obviously works and even seems a bit simpler than the old code, but I'm a bit confused how we were ending up in this state.

---

_@chirizxc reviewed on 2025-10-09 17:49_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:176 on 2025-10-09 17:49_

I didn't quite get it, but in `make_suggestion` we actually just cut the source code for the argument (locator.slice(arg.range())) and put it directly, condition with open.keywords is needed to keep all arguments with names that were passed to the original open(...)

---

_Review requested from @ntBre by @MichaReiser on 2025-10-12 06:45_

---

_@MichaReiser reviewed on 2025-10-13 08:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:176 on 2025-10-13 08:05_

The issue here is that the generator emits the arguments (and keyword arguments) in source order. See here

https://github.com/astral-sh/ruff/blob/48ada2d359b7c985454f4ac7ae7f23d1e980865a/crates/ruff_python_codegen/src/generator.rs#L1191-L1209

This is obviously very surprising and probably another reason why we should change `arguments` to be a single vec containing both keywoard and normal arguments, so that we don't have to do this guessing game inside generator.

For this rule, I think the fix is to use a more proper range for `arg` that isn't `TextRange::default` and ensures that the argument comes in the right order.


---

_@chirizxc reviewed on 2025-10-13 12:31_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:176 on 2025-10-13 12:31_

> The issue here is that the generator emits the arguments (and keyword arguments) in source order. See here
> 
> https://github.com/astral-sh/ruff/blob/48ada2d359b7c985454f4ac7ae7f23d1e980865a/crates/ruff_python_codegen/src/generator.rs#L1191-L1209
> 
> This is obviously very surprising and probably another reason why we should change `arguments` to be a single vec containing both keywoard and normal arguments, so that we don't have to do this guessing game inside generator.
> 
> For this rule, I think the fix is to use a more proper range for `arg` that isn't `TextRange::default` and ensures that the argument comes in the right order.

I think this solution should be optimal, since we get rid of AST transformations and `arg.clone()`, and we also preserve the original spaces, please correct if this is wrong

<img width="1894" height="555" alt="изображение" src="https://github.com/user-attachments/assets/b6f08af2-4423-48f8-b374-9e0b125e4908" />


---

_@MichaReiser reviewed on 2025-10-20 08:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:176 on 2025-10-20 08:22_

I agree that it ensures that we preserve the formatting. My main concern is that switching from AST to text edits often leads to introducing different bugs (e.g. where precedence changes, the precense of comments or trailing commas or something else leads to invalid syntax). That's why I prefer to keep fixes as minimal as possible, to reduce the risk of breaking something else. In this case, this would mean to stick to AST based edits.

---

_@ntBre reviewed on 2025-10-20 16:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:176 on 2025-10-20 16:02_

I agree with Micha, I'd at least like to try improving the `arg` we're passing here before taking the current approach. It still feels like we're just addressing the symptom, but the underlying cause could still cause other problems and is confusing anyway.

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:176 on 2025-10-23 21:16_

> I agree with Micha, I'd at least like to try improving the `arg` we're passing here before taking the current approach. It still feels like we're just addressing the symptom, but the underlying cause could still cause other problems and is confusing anyway.

I tried several times with the previous version, but nothing works.

When we call `relocate_expr(&mut arg, ...)` on a complex nested expression like `json.dumps(data, indent=4).encode("utf-8")`, the function recursively updates ALL nested nodes while preserving their relative positions. This means:
* The outer .encode() call gets relocated to range [0, arg_len)
* The inner `json.dumps(...)` call gets a range within [0, X) where X < arg_len
* Arguments inside `json.dumps`: data gets [Y, Z), indent=4 gets [A, B) - all within the relocated range.

`arguments_source_order()` sorts ALL arguments from ALL function calls in the entire AST tree by their TextRange::start() position . Since the nested indent=4 keyword has a smaller start offset than data in the original source, it remains before data even after relocation, producing invalid syntax: json.dumps(indent=4, data).

I understood it this way, maybe I'm wrong🤷‍♂️

---

_@chirizxc reviewed on 2025-10-23 21:16_

---

_@ntBre reviewed on 2025-10-30 22:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:176 on 2025-10-30 22:20_

Thanks for writing this up! I took a look today, and I think you're right. `relocate_expr` doesn't just shift the `json.dumps` call, it recurses and relocates all of the child nodes, including all of its arguments. All of the inputs are correct here, but any range we pass to `relocate_expr` will have the same issue.

One thing that seems to help is to make the comparison in `arguments_source_order` `<=` instead of just `<`. I think that should avoid reordering equal elements, as we have here after `relocate_expr`. That worked locally for me with no other regressions after reverting the other non-test changes in this PR.

Other than that, I think we would need a non-recursive variant of `relocate_expr`. The only other use of `relocate_expr` is for relocating type expressions that we parse from strings, which seems a bit different and might actually need the recursion (https://github.com/astral-sh/ruff/pull/133).

<details><summary>Patch (mostly revert + one character in nodes.rs)</summary>
<p>

```diff
diff --git a/crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs b/crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs
index b46008ff8c..bbee6dcb5a 100644
--- a/crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs
+++ b/crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs
@@ -2,15 +2,18 @@ use ruff_diagnostics::{Applicability, Edit, Fix};
 use ruff_macros::{ViolationMetadata, derive_message_formats};
 use ruff_python_ast::{
     self as ast, Expr, Stmt,
+    relocate::relocate_expr,
     visitor::{self, Visitor},
 };
-use ruff_text_size::Ranged;
+
+use ruff_python_codegen::Generator;
+use ruff_text_size::{Ranged, TextRange};
 
 use crate::checkers::ast::Checker;
 use crate::fix::snippet::SourceCodeSnippet;
 use crate::importer::ImportRequest;
 use crate::rules::refurb::helpers::{FileOpen, find_file_opens};
-use crate::{FixAvailability, Locator, Violation};
+use crate::{FixAvailability, Violation};
 
 /// ## What it does
 /// Checks for uses of `open` and `write` that can be replaced by `pathlib`
@@ -127,7 +130,7 @@ impl<'a> Visitor<'a> for WriteMatcher<'a, '_> {
                 let open = self.candidates.remove(open);
 
                 if self.loop_counter == 0 {
-                    let suggestion = make_suggestion(&open, content, self.checker.locator());
+                    let suggestion = make_suggestion(&open, content, self.checker.generator());
 
                     let mut diagnostic = self.checker.report_diagnostic(
                         WriteWholeFile {
@@ -163,6 +166,7 @@ fn match_write_call(expr: &Expr) -> Option<(&Expr, &Expr)> {
     let method_name = &attr.attr;
 
     if method_name != "write"
+        || !attr.value.is_name_expr()
         || call.arguments.args.len() != 1
         || !call.arguments.keywords.is_empty()
     {
@@ -173,21 +177,27 @@ fn match_write_call(expr: &Expr) -> Option<(&Expr, &Expr)> {
     Some((&*attr.value, call.arguments.args.first()?))
 }
 
-fn make_suggestion(open: &FileOpen<'_>, arg: &Expr, locator: &Locator) -> String {
-    let method_name = open.mode.pathlib_method();
-    let arg_code = locator.slice(arg.range());
-
-    if open.keywords.is_empty() {
-        format!("{method_name}({arg_code})")
-    } else {
-        format!(
-            "{method_name}({arg_code}, {})",
-            itertools::join(
-                open.keywords.iter().map(|kw| locator.slice(kw.range())),
-                ", "
-            )
-        )
-    }
+fn make_suggestion(open: &FileOpen<'_>, arg: &Expr, generator: Generator) -> String {
+    let name = ast::ExprName {
+        id: open.mode.pathlib_method(),
+        ctx: ast::ExprContext::Load,
+        range: TextRange::default(),
+        node_index: ruff_python_ast::AtomicNodeIndex::NONE,
+    };
+    let mut arg = arg.clone();
+    relocate_expr(&mut arg, TextRange::default());
+    let call = ast::ExprCall {
+        func: Box::new(name.into()),
+        arguments: ast::Arguments {
+            args: Box::new([arg]),
+            keywords: open.keywords.iter().copied().cloned().collect(),
+            range: TextRange::default(),
+            node_index: ruff_python_ast::AtomicNodeIndex::NONE,
+        },
+        range: TextRange::default(),
+        node_index: ruff_python_ast::AtomicNodeIndex::NONE,
+    };
+    generator.expr(&call.into())
 }
 
 fn generate_fix(
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index f71f420d09..5cb58e7f05 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -3372,7 +3372,7 @@ impl Arguments {
     pub fn arguments_source_order(&self) -> impl Iterator<Item = ArgOrKeyword<'_>> {
         let args = self.args.iter().map(ArgOrKeyword::Arg);
         let keywords = self.keywords.iter().map(ArgOrKeyword::Keyword);
-        args.merge_by(keywords, |left, right| left.start() < right.start())
+        args.merge_by(keywords, |left, right| left.start() <= right.start())
     }
 
     pub fn inner_range(&self) -> TextRange {
```

</p>
</details> 

---

_@ntBre approved on 2025-10-31 15:14_

Thank you!

---

_Label `preview` added by @ntBre on 2025-10-31 15:14_

---

_Renamed from "[`refurb`] Fix `FURB103` fixes" to "[`refurb`] Preserve argument ordering in autofix (`FURB103`)" by @ntBre on 2025-10-31 15:16_

---

_Merged by @ntBre on 2025-10-31 15:16_

---

_Closed by @ntBre on 2025-10-31 15:16_

---

_Branch deleted on 2025-10-31 15:35_

---
