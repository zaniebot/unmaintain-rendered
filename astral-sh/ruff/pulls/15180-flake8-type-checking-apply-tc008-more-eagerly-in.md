```yaml
number: 15180
title: "[`flake8-type-checking`] Apply `TC008` more eagerly in `TYPE_CHECKING` blocks and disapply it in stubs"
type: pull_request
state: merged
author: Daverball
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/tc008-typing-execution-context
created_at: 2024-12-29T08:34:55Z
updated_at: 2025-01-08T13:23:45Z
url: https://github.com/astral-sh/ruff/pull/15180
synced_at: 2026-01-12T15:55:50Z
```

# [`flake8-type-checking`] Apply `TC008` more eagerly in `TYPE_CHECKING` blocks and disapply it in stubs

---

_@Daverball_

## Summary

`quoted-type-alias` (`TC008`) can be applied more liberally for annotated type aliases defined inside a `if TYPE_CHECKING` block.

This also disables `TC008` in stub files, since that use-case is already covered by `quoted-annotation-in-stub` (`PYI020`).

## Test Plan

`cargo nextest run`


---

_Comment by @Daverball on 2024-12-29 08:53_

The stub test case will fail until #15179 is merged, due to the circularity issues with TC007, when TC007 isn't skipped in stub files.

---

_Comment by @github-actions[bot] on 2024-12-29 21:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/decorators/condition.py#L32'>airflow/decorators/condition.py:32:35:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/decorators/condition.py#L33'>airflow/decorators/condition.py:33:35:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/ldata/_transfer/upload.py#L30'>src/latch/ldata/_transfer/upload.py:30:32:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/ldata/_transfer/upload.py#L31'>src/latch/ldata/_transfer/upload.py:31:35:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC008 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @Daverball on 2024-12-29 21:52_

Alright, this is a little too liberal right now. I had my suspicions.

I'll go more conservative and only remove the subscript/attribute exception. Recursive type aliases should also result in TC008 in a typing only context, but it might be too much work to carve out that exception.

---

_Comment by @Daverball on 2024-12-29 22:35_

The remaining ecosystem hits look correct to me.

---

_Comment by @Daverball on 2024-12-29 22:41_

Actually, we probably need to carve out an additional exception for `|` if the target version is < 3.10 and we're not inside a stub file.

But that's getting out of scope, since it has nothing to do with execution context. I'll create a follow-up PR for that.

---

_@MichaReiser reviewed on 2024-12-30 17:37_

Thank you. 

Do you have any references that I could read why it is okay to apply TC008 more eagerly in typing only context? Having a more detailed explanation might also be useful for users reading the Changelog

---

_Comment by @Daverball on 2024-12-30 18:46_

The exceptions TC008 currently carves out for attribute and subscript access only are there because some types will only be subscriptable at type checking time (E.g. a class is made generic in a `pyi` file, but the class in the corresponding `py` file is not generic, so it can't be subscripted at runtime) and some attributes will be internal to the stubs (E.g. convenience type aliases like `webob.request._FieldStorageWithFile`, if you import `webob.request` and access the attribute as `webob.request._FieldStorageWithFile` at runtime, it will be a `NameError`).

Neither of these cases are relevant in typing only contexts like type checking blocks or stub files. Since they will never be evaluated by CPython, only by static analysis tools, which includes most if not all runtime type checkers. So we only really need to adhere to what type checkers consider correct.

---

_Comment by @Daverball on 2024-12-30 18:54_

As for references. I don't think this is something that's explicitly specced out anywhere, except maybe in some specific type checker's documentation. These are just some nuances/heuristics I have learned over the course of many hundreds of hours of working with mypy and to some degree pyright and pytype via typeshed contributions.

Once ruff performs deeper analysis it might be possible to detect some of these errors without having to carve out large sweeping exceptions like these two. Although it would involve analyzing both the stubs and the actual source for modules with external stubs.

---

_Comment by @MichaReiser on 2024-12-31 14:25_

Hmm okay, thanks for the explanation. This does make sense but I think it might be better to wait for @AlexWaygood or @carljm to review this change. 

I was interested whether this would impact runtime type checkers but it seems they replace all types declared in `TYPE_CHECKING` blocks with `Any` ([source](https://typeguard.readthedocs.io/en/latest/userguide.html#notes-on-forward-reference-handling))

---

_Assigned to @AlexWaygood by @MichaReiser on 2024-12-31 14:25_

---

_Label `rule` added by @MichaReiser on 2024-12-31 14:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-03 09:37_

---

_Comment by @AlexWaygood on 2025-01-03 11:26_

@Daverball before I review this PR, could you clarify this a little bit for me:

> Actually, we probably need to carve out an additional exception for `|` if the target version is < 3.10 and we're not inside a stub file.
> 
> But that's getting out of scope, since it has nothing to do with execution context. I'll create a follow-up PR for that.

Does this PR _introduce_ a false positive that you then plan to fix in https://github.com/astral-sh/ruff/pull/15201? Or does this false positive that you identify here already exist on `main`?

If the former, then I think the false positive should be fixed in this PR, because otherwise there's a chance that we review and merge this PR, then look at your other PR, decide it's not the correct fix, and we're stuck with a false positive on `main` for a prolonged period of time. But if the latter, then I'm happy for things to be split into two PRs, as you've done :-)

---

_Comment by @Daverball on 2025-01-03 11:50_

@AlexWaygood Yes, the false positive already exists on main, otherwise I would've fixed it in this PR. I just happened to notice the bug while I was implementing this.

---

_Comment by @AlexWaygood on 2025-01-03 11:54_

Great, thanks!

---

_@AlexWaygood requested changes on 2025-01-03 12:16_

This preview rule seems to already overlap with our stable rule [`PYI020`](https://docs.astral.sh/ruff/rules/quoted-annotation-in-stub/). If you run `uvx ruff check --select=PYI,TC --preview` on this snippet saved as a `.pyi` file, you already get two diagnostics (one from PYI20, one from TC008) using the latest version of Ruff:

```py
type Y = "list[str]"
```

This PR makes the problem worse; with this PR branch, running `cargo run -- check foo.pyi --select=TC,PYI --preview --no-cache` means that the problem is extended to old-style type aliases. Now we get duplicate diagnostics on this as well in a stub-file context:

```py
from typing import TypeAlias

X: TypeAlias = "list[str]"
```

We'll need to decide how to resolve the overlap between the two rules before we can consider stabilising `TC008`. I think PYI020` is likely to be a rule that stub authors will already have enabled (since it's already stable, and since the whole category is focussed on writing better stubs), so my instinct is to say that it would be best if TC008 did not emit any lints at all regarding stub files. That feels like the way of resolving the overlap between the rules that will be least disruptive to users.

This is obviously not a perfect solution. In an ideal world, I think we'd have a single rule (probably `TC008`) that just adjusted its behaviour depending on whether it was analyzing a stub file or a runtime file (what you're doing now). Perhaps that's something we could do as part of rule recategorisation. As it stands, however, this would require deprecating PYI020. That feels unnecessarily disruptive to me, considering that it's a stable rule with no known problems in its implementation.

---

_Comment by @Daverball on 2025-01-03 12:34_

@AlexWaygood I'm fine with disabling this rule in stub files, if the use-case is already served by `flake8-pyi`, that simplifies some things too.

---

_Comment by @Daverball on 2025-01-03 13:00_

@AlexWaygood Maybe this means we want to disable TC010 in stubs as well. Although the overlap there should be much smaller, if there is one. It only seems to apply to type expressions that are contained within value expressions, like a `cast` or a generic subscript in the rhs of an assignment or a function default. And it's also possible that PYI020 doesn't apply in that specific context, so there isn't actually any overlap.

---

_Renamed from "[`flake8-type-checking`] Apply `TC008` more eagerly in typing context" to "[`flake8-type-checking`] Apply `TC008` more eagerly in `TYPE_CHECKING` blocks" by @Daverball on 2025-01-03 13:01_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:2328 on 2025-01-03 13:05_

Let's add a comment so it's obvious to future readers why this rule is disabled on stub files:

```suggestion
                        // stub files are covered by PYI020
                        if !self.source_type.is_stub() && self.enabled(Rule::QuotedTypeAlias) {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:1 on 2025-01-03 13:06_

Could we also add a note to the docs for this rule pointing users to PYI020 for stub files? (And we could similarly add a note to the PYI020 docs pointing users to this rule for runtime files)

---

_Review requested from @AlexWaygood by @Daverball on 2025-01-03 13:14_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:219 on 2025-01-03 13:19_

The issue with boolean parameters in Rust is that, because the language doesn't have keyword arguments, it's impossible to know what the passed-in argument means without jumping to the function's definition (which is kind of a pain). Using a custom enum for this parameter in the methods in the semantic model would make this much more readable at callsites such as this one, e.g.

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs b/crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs
index 92b1d7d07..cc663022d 100644
--- a/crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs
+++ b/crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs
@@ -4,7 +4,7 @@ use ruff_diagnostics::{AlwaysFixableViolation, Diagnostic, Edit, Fix, FixAvailab
 use ruff_macros::{derive_message_formats, ViolationMetadata};
 use ruff_python_ast as ast;
 use ruff_python_ast::{Expr, Stmt};
-use ruff_python_semantic::{Binding, SemanticModel};
+use ruff_python_semantic::{Binding, SemanticModel, TypingOnlyBindingsStatus};
 use ruff_python_stdlib::typing::{is_pep_593_generic_type, is_standard_library_literal};
 use ruff_text_size::Ranged;
 
@@ -221,7 +221,7 @@ fn collect_typing_references<'a>(
             };
             if checker
                 .semantic()
-                .simulate_runtime_load(name, false)
+                .simulate_runtime_load(name, TypingOnlyBindingsStatus::Disallowed)
                 .is_some()
             {
                 return;
@@ -322,7 +322,7 @@ fn quotes_are_unremovable(semantic: &SemanticModel, expr: &Expr) -> bool {
         Expr::Name(name) => {
             semantic.resolve_name(name).is_some()
                 && semantic
-                    .simulate_runtime_load(name, semantic.in_type_checking_block())
+                    .simulate_runtime_load(name, semantic.in_type_checking_block().into())
                     .is_none()
         }
         _ => false,
diff --git a/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs b/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs
index 6678b90d6..186b9636e 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs
@@ -7,7 +7,7 @@ use ruff_python_ast as ast;
 use ruff_python_literal::format::FormatSpec;
 use ruff_python_parser::parse_expression;
 use ruff_python_semantic::analyze::logging::is_logger_candidate;
-use ruff_python_semantic::{Modules, SemanticModel};
+use ruff_python_semantic::{Modules, SemanticModel, TypingOnlyBindingsStatus};
 use ruff_text_size::{Ranged, TextRange};
 
 use crate::checkers::ast::Checker;
@@ -216,7 +216,7 @@ fn should_be_fstring(
                         id,
                         literal.range(),
                         semantic.scope_id,
-                        false,
+                        TypingOnlyBindingsStatus::Disallowed,
                     )
                     .map_or(true, |id| semantic.binding(id).kind.is_builtin())
                 {
diff --git a/crates/ruff_python_semantic/src/model.rs b/crates/ruff_python_semantic/src/model.rs
index ae8997158..56cd21ddb 100644
--- a/crates/ruff_python_semantic/src/model.rs
+++ b/crates/ruff_python_semantic/src/model.rs
@@ -713,13 +713,13 @@ impl<'a> SemanticModel<'a> {
     pub fn simulate_runtime_load(
         &self,
         name: &ast::ExprName,
-        allow_typing_only_bindings: bool,
+        typing_only_bindings_status: TypingOnlyBindingsStatus,
     ) -> Option<BindingId> {
         self.simulate_runtime_load_at_location_in_scope(
             name.id.as_str(),
             name.range,
             self.scope_id,
-            allow_typing_only_bindings,
+            typing_only_bindings_status,
         )
     }
 
@@ -752,7 +752,7 @@ impl<'a> SemanticModel<'a> {
         symbol: &str,
         symbol_range: TextRange,
         scope_id: ScopeId,
-        allow_typing_only_bindings: bool,
+        typing_only_bindings_status: TypingOnlyBindingsStatus,
     ) -> Option<BindingId> {
         let mut seen_function = false;
         let mut class_variables_visible = true;
@@ -795,7 +795,9 @@ impl<'a> SemanticModel<'a> {
                     // runtime binding with a source-order inaccurate one
                     for shadowed_id in scope.shadowed_bindings(binding_id) {
                         let binding = &self.bindings[shadowed_id];
-                        if !allow_typing_only_bindings && binding.context.is_typing() {
+                        if typing_only_bindings_status.is_disallowed()
+                            && binding.context.is_typing()
+                        {
                             continue;
                         }
                         if let BindingKind::Annotation
@@ -830,7 +832,7 @@ impl<'a> SemanticModel<'a> {
                         _ => binding_id,
                     };
 
-                    if !allow_typing_only_bindings
+                    if typing_only_bindings_status.is_disallowed()
                         && self.bindings[candidate_id].context.is_typing()
                     {
                         continue;
@@ -2065,6 +2067,32 @@ impl ShadowedBinding {
     }
 }
 
+#[derive(Debug, Clone, Copy, PartialEq, Eq)]
+pub enum TypingOnlyBindingsStatus {
+    Allowed,
+    Disallowed,
+}
+
+impl TypingOnlyBindingsStatus {
+    pub const fn is_allowed(self) -> bool {
+        matches!(self, TypingOnlyBindingsStatus::Allowed)
+    }
+
+    pub const fn is_disallowed(self) -> bool {
+        matches!(self, TypingOnlyBindingsStatus::Disallowed)
+    }
+}
+
+impl From<bool> for TypingOnlyBindingsStatus {
+    fn from(value: bool) -> Self {
+        if value {
+            TypingOnlyBindingsStatus::Allowed
+        } else {
+            TypingOnlyBindingsStatus::Disallowed
+        }
+    }
+}
+
```

---

_@AlexWaygood reviewed on 2025-01-03 13:21_

Thanks for updating this! It overall looks reasonable to me now

---

_Comment by @AlexWaygood on 2025-01-03 13:22_

> @AlexWaygood Maybe this means we want to disable TC010 in stubs as well. Although the overlap there should be much smaller, if there is one. It only seems to apply to type expressions that are contained within value expressions, like a `cast` or a generic subscript in the rhs of an assignment or a function default. And it's also possible that PYI020 doesn't apply in that specific context, so there isn't actually any overlap.

PYI020 bans all quotes in all type expressions in stubs (with very narrow exceptions for `Literal` and `Annotated`), so yes, it looks like there's overlap there as well, which we should fix.

---

_@Daverball reviewed on 2025-01-03 13:43_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:1 on 2025-01-03 13:43_

@AlexWaygood Do you happen to know how to cross reference rules from another rule? I couldn't find any examples. Does linking to the corresponding enum variant work?

---

_@AlexWaygood reviewed on 2025-01-03 14:30_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:1 on 2025-01-03 14:30_

Hmmm... no, I'm not sure what the best solution is there. @MichaReiser, do you have any guidance/insight?

---

_@AlexWaygood reviewed on 2025-01-06 11:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:1 on 2025-01-06 11:16_

I chatted to @MichaReiser this morning and we concluded that neither of us knows of a great way to do this :-) hardcoding a link to https://docs.astral.sh/ruff/rules/quoted-annotation-in-stub/ might be best for now

---

_@MichaReiser reviewed on 2025-01-06 11:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:1 on 2025-01-06 11:42_

I just double checked and yes, there's no custom handling in https://github.com/astral-sh/ruff/blob/bb12fe9d0c71fcb36a5000260f62dbf8411b74b4/crates/ruff_dev/src/generate_docs.rs#L118-L187

It might be possible to use relative links as we do in the linter documentation

https://github.com/astral-sh/ruff/blob/68eb0a25111c23b0891afb07e68e785978963904/docs/linter.md#L165-L167

---

_@Daverball reviewed on 2025-01-06 11:43_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:1 on 2025-01-06 11:43_

I could probably also link `/rules/quoted-annotation-in-stub.md` so it's only semi-hardlinked, I had to do something similar in a setting to link to a rule.

---

_@Daverball reviewed on 2025-01-06 12:16_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:1 on 2025-01-06 12:16_

@AlexWaygood I think a reference from TC008 to PYI020 is sufficient. A reference the other way doesn't make that much sense to me, since most PYI rules are so specific to stub files.

---

_Review requested from @AlexWaygood by @Daverball on 2025-01-06 12:21_

---

_Comment by @Daverball on 2025-01-08 09:15_

@AlexWaygood Bumping this in case it slipped through the cracks.

---

_@AlexWaygood approved on 2025-01-08 12:05_

Thanks!

---

_Renamed from "[`flake8-type-checking`] Apply `TC008` more eagerly in `TYPE_CHECKING` blocks" to "[`flake8-type-checking`] Apply `TC008` more eagerly in `TYPE_CHECKING` blocks and disapply it in stubs" by @AlexWaygood on 2025-01-08 12:05_

---

_Merged by @AlexWaygood on 2025-01-08 12:09_

---

_Closed by @AlexWaygood on 2025-01-08 12:09_

---

_Branch deleted on 2025-01-08 13:23_

---
