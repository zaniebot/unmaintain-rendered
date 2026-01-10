```yaml
number: 18489
title: "[ty] remove unnecessary Either"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/remove-unnecessary-either
created_at: 2025-06-06T01:24:11Z
updated_at: 2025-06-07T02:19:33Z
url: https://github.com/astral-sh/ruff/pull/18489
synced_at: 2026-01-10T18:45:04Z
```

# [ty] remove unnecessary Either

---

_Pull request opened by @carljm on 2025-06-06 01:24_

Just a quick review-comment follow-up.


---

_Review requested from @AlexWaygood by @carljm on 2025-06-06 01:24_

---

_Label `ty` added by @carljm on 2025-06-06 01:24_

---

_Review requested from @sharkdp by @carljm on 2025-06-06 01:24_

---

_Review requested from @dcreager by @carljm on 2025-06-06 01:24_

---

_Comment by @github-actions[bot] on 2025-06-06 01:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @carljm on 2025-06-06 01:39_

---

_Closed by @carljm on 2025-06-06 01:39_

---

_Branch deleted on 2025-06-06 01:39_

---

_Comment by @AlexWaygood on 2025-06-06 16:41_

It could be even simpler, FWIW...

```diff
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -9021,9 +9021,9 @@ impl<'db> TypeInferenceBuilder<'db, '_> {
     ) -> Type<'db> {
         let arguments = &*subscript_node.slice;
         let (args, args_number) = if let ast::Expr::Tuple(t) = arguments {
-            (t.iter(), t.len())
+            (&*t.elts, t.len())
         } else {
-            (std::slice::from_ref(arguments).iter(), 1)
+            (std::slice::from_ref(arguments), 1)
         };
         if args_number != expected_arg_count {
             if let Some(builder) = self.context.report_lint(&INVALID_TYPE_FORM, subscript_node) {
@@ -9040,8 +9040,7 @@ impl<'db> TypeInferenceBuilder<'db, '_> {
         }
         let ty = class.to_specialized_instance(
             self.db(),
-            args.into_iter()
-                .map(|node| self.infer_type_expression(node)),
+            args.iter().map(|node| self.infer_type_expression(node)),
         );
         if arguments.is_tuple_expr() {
             self.store_expression_type(arguments, ty);
```

...but I don't think it's worth a followup PR just for this :P

---

_Comment by @MichaReiser on 2025-06-06 16:57_

In which case you could even remove the `len` because of `slice.len` ;)

Maybe something for claude

---

_Comment by @carljm on 2025-06-07 02:18_

PRs are cheap :) https://github.com/astral-sh/ruff/pull/18526

And this is too simple for Claude -- it'd be way more work to explain it to Claude than to just make the change.

---

_Label `internal` added by @carljm on 2025-06-07 02:19_

---
