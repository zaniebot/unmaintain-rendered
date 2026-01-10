```yaml
number: 15976
title: "[red-knot] fix unresolvable import range"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/fix-unresolved-important-range
created_at: 2025-02-05T18:10:44Z
updated_at: 2025-02-05T19:01:59Z
url: https://github.com/astral-sh/ruff/pull/15976
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] fix unresolvable import range

---

_Pull request opened by @BurntSushi on 2025-02-05 18:10_

This causes the diagnostic to highlight the actual unresovable import
instead of the entire `from ... import ...` statement.

While we're here, we expand the test coverage to cover all of the
possible ways that an `import` or a `from ... import` can fail.

Some considerations:

* The first commit in this PR adds a regression test for the current
behavior.
* This creates a new `mdtest/diagnostics` directory. Are folks cool
with this? I guess the idea is to put tests more devoted to diagnostics
than semantics in this directory. (Although I'm guessing there will
be some overlap.)

Fixes astral-sh/ruff#15866


---

_Review requested from @carljm by @BurntSushi on 2025-02-05 18:10_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-02-05 18:10_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-02-05 18:10_

---

_Review requested from @sharkdp by @BurntSushi on 2025-02-05 18:10_

---

_Comment by @BurntSushi on 2025-02-05 18:11_

I am already pining for inline snapshots. The external snapshot approach completely kills the amazing locality that mdtests otherwise benefit from. Sigh.

---

_@sharkdp reviewed on 2025-02-05 18:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:20_

Minor: It looks like we might want to highlight the full module path here (it seems to not include the leading `.`)? Maybe we can test this with a `longer.module.path`?

Edit: I guess ideally, we would only highlight the part of the module path that does not exist. But that might be much more difficult to implement (I'm not at all familiar with that part of the code base).

---

_Label `red-knot` added by @AlexWaygood on 2025-02-05 18:24_

---

_@BurntSushi reviewed on 2025-02-05 18:24_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:24_

I added a test with submodules and the range does include them. It looks like it's just the leading `.`. I'll investigate and see if I can cover that.

---

_@sharkdp reviewed on 2025-02-05 18:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_a_resolvable_module_but_unresolvable_item.snap`:30 on 2025-02-05 18:25_

Can we extend this to include one existing symbol? Such that it would read:

```suggestion
1 | from a import does_exist, does_not_exist  # error: [unresolved-import]
  |                           ^^^^^^^^^^^^^^ Module `a` has no member `does_not_exist`
```

---

_@sharkdp approved on 2025-02-05 18:26_

Thank you!

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_a_resolvable_module_but_unresolvable_item.snap`:30 on 2025-02-05 18:27_

Done.

---

_@BurntSushi reviewed on 2025-02-05 18:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/unresolved-import.md`:1 on 2025-02-05 18:28_

tiny nit: our naming convention for mdtests is to use snake_case rather than kebab-case. So this should probably be `unresolved_import.md` rather than `unresolved-import.md`

---

_@AlexWaygood reviewed on 2025-02-05 18:28_

---

_@BurntSushi reviewed on 2025-02-05 18:32_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:32_

Yeah this one looks trickier because it looks like the AST normalizes the dots to a `level` number:

```
[crates/red_knot_python_semantic/src/types/infer.rs:748:17] &import_from = ImportFromDefinitionKind {
    node: AstNodeRef(
        StmtImportFrom {
            range: 0..24,
            module: Some(
                Identifier {
                    id: Name("zqzqzqzq"),
                    range: 6..14,
                },
            ),
            names: [
                Alias {
                    range: 22..24,
                    name: Identifier {
                        id: Name("hi"),
                        range: 22..24,
                    },
                    asname: None,
                },
            ],
            level: 1,
        },
    ),
    alias_index: 0,
}
```

So it's not totally clear (to me) how to fix this particular problem without changes to the AST. (Or working backwards to try and figure out the range by looking at the level and the actual source code. But that feels kinda bad.)

---

_@BurntSushi reviewed on 2025-02-05 18:35_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:35_

Yeah the "cannot resolve import `.foo`" in the diagnostic message is being re-constituted here:

https://github.com/astral-sh/ruff/blob/3cf9cd5d475c475adee40fc05443a5fe2f6a3494/crates/red_knot_python_semantic/src/types/diagnostic.rs#L971-L975

And you wind up with source code in the diagnostic that isn't actually in the source program. For example:

```
[andrew@duff play]$ cat test.py
from . . . zqzqzqzq import hi
[andrew@duff play]$ run-red-knot pr1 -- check
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.08s
error: lint:unresolved-import
 --> /home/andrew/astral/ruff/play/diags/play/test.py:1:12
  |
1 | from . . . zqzqzqzq import hi
  |            ^^^^^^^^ Cannot resolve import `...zqzqzqzq`
  |

```

I'll file a bug for this one but punt on it here.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:35_

I think it wouldn't be too _hard_ to fix if we changed `InferContext::report_diagnostic` and `InferContext::report_lint` to take a `TextRange` rather than an `AnyNodeRef`:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/context.rs b/crates/red_knot_python_semantic/src/types/context.rs
index 00e63192b..3077a63a9 100644
--- a/crates/red_knot_python_semantic/src/types/context.rs
+++ b/crates/red_knot_python_semantic/src/types/context.rs
@@ -6,7 +6,7 @@ use ruff_db::{
     files::File,
 };
 use ruff_python_ast::AnyNodeRef;
-use ruff_text_size::Ranged;
+use ruff_text_size::{Ranged, TextRange};
 
 use super::{binding_type, KnownFunction, TypeCheckDiagnostic, TypeCheckDiagnostics};
 
@@ -71,7 +71,7 @@ impl<'db> InferContext<'db> {
     pub(super) fn report_lint(
         &self,
         lint: &'static LintMetadata,
-        node: AnyNodeRef,
+        range: TextRange,
         message: fmt::Arguments,
     ) {
         if !self.db.is_file_open(self.file) {
@@ -89,12 +89,12 @@ impl<'db> InferContext<'db> {
 
         let suppressions = suppressions(self.db, self.file);
 
-        if let Some(suppression) = suppressions.find_suppression(node.range(), LintId::of(lint)) {
+        if let Some(suppression) = suppressions.find_suppression(range, LintId::of(lint)) {
             self.diagnostics.borrow_mut().mark_used(suppression.id());
             return;
         }
 
-        self.report_diagnostic(node, DiagnosticId::Lint(lint.name()), severity, message);
+        self.report_diagnostic(range, DiagnosticId::Lint(lint.name()), severity, message);
     }
 
     /// Adds a new diagnostic.
@@ -102,7 +102,7 @@ impl<'db> InferContext<'db> {
     /// The diagnostic does not get added if the rule isn't enabled for this file.
     pub(super) fn report_diagnostic(
         &self,
-        node: AnyNodeRef,
+        range: TextRange,
         id: DiagnosticId,
         severity: Severity,
         message: fmt::Arguments,
@@ -121,7 +121,7 @@ impl<'db> InferContext<'db> {
             file: self.file,
             id,
             message: message.to_string(),
-            range: node.range(),
+            range,
             severity,
         });
     }
```

And then we could just fixup the `TextRange` passed to `InferContext` here by subtracting `level` from the start of the `import_node` range here:

https://github.com/astral-sh/ruff/blob/a84b27e679137697bdb994e9c5f79d366fb4a945/crates/red_knot_python_semantic/src/types/diagnostic.rs#L962-L977

But changing the signatures of `InferContext::report_diagnostic` and `InferContext::report_lint` requires updating many callsites

---

_@AlexWaygood reviewed on 2025-02-05 18:35_

---

_@AlexWaygood reviewed on 2025-02-05 18:38_

---

_@BurntSushi reviewed on 2025-02-05 18:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:38_

(TL;DR: punting on this for now sounds like the correct decision to me)

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/unresolved-import.md`:1 on 2025-02-05 18:38_

Booo okay, fixed haha.

---

_@BurntSushi reviewed on 2025-02-05 18:41_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:41_

Yeah but if you subtract `level` then you're assuming there isn't any whitespace between the dots right? See above where I did `from . . . foo import wat`, and it looks like the parsed level is still `3`, but tweaking a range by assuming 3 characters would be wrong there.

---

_@AlexWaygood reviewed on 2025-02-05 18:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:46_

Oh wow, I didn't even know you were allowed to have whitespace there :-( Yup, that wrecks everything...

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:46_

Filed here https://github.com/astral-sh/ruff/issues/15977

---

_@BurntSushi reviewed on 2025-02-05 18:46_

---

_@BurntSushi reviewed on 2025-02-05 18:47_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:47_

Yeah I didn't either. But I'm used to these sorts of problems with ASTs which made me try it haha.

---

_@AlexWaygood reviewed on 2025-02-05 18:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/unresolved-import.md_-_Unresolved_import_diagnostics_-_Using_`from`_with_an_unknown_current_module.snap`:27 on 2025-02-05 18:49_

whitespace: it's significant in Python! except when it isn't 🙃

---

_@AlexWaygood approved on 2025-02-05 18:55_

LGTM

> This creates a new mdtest/diagnostics directory. Are folks cool with this? I guess the idea is to put tests more devoted to diagnostics than semantics in this directory. (Although I'm guessing there will be some overlap.)

I feel like my preference is for tests to be organised in terms of the Python semantics they're covering (so all the import-related tests would be together, even if some are specifically testing our type inference and some are specifically testing what the diagnostics actually look like). But this is a _very_ weakly held opinion, in part because I already find it quite hard to figure out where the test for something or other will be found in our many mdtest (sub)directories 😄

---

_Comment by @BurntSushi on 2025-02-05 18:57_

Yeah I hear that. What I was going for, at least initially here, was to organize diagnostic tests by their diagnostic ID.

(Whenever I've done stuff like this in the past, I almost always discover later that my initial instincts for organization were wrong and end up revising them. So I'm very happy to adjust as we learn more here.)

---

_Label `diagnostics` added by @AlexWaygood on 2025-02-05 18:57_

---

_Merged by @BurntSushi on 2025-02-05 19:01_

---

_Closed by @BurntSushi on 2025-02-05 19:01_

---

_Branch deleted on 2025-02-05 19:01_

---
