```yaml
number: 21029
title: "[ty] Set `INSTA_FORCE_PASS` and `INSTA_OUTPUT` environment variables from mdtest.py"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/mdtest-snapshot-integration2
created_at: 2025-10-22T13:58:40Z
updated_at: 2025-10-22T14:32:16Z
url: https://github.com/astral-sh/ruff/pull/21029
synced_at: 2026-01-12T15:57:14Z
```

# [ty] Set `INSTA_FORCE_PASS` and `INSTA_OUTPUT` environment variables from mdtest.py

---

_@AlexWaygood_

## Summary

Setting these environment variables makes it much less painful to iterate on an mdtest that has snapshots enabled.

Currently if any snapshots change, you can't see which mdtest assertions are failing until you accept the snapshots (which requires terminating mdtest.py). So you accept the snapshots, then start mdtest.py running again, then you find some mdtest assertions are failing, so then you update them... and now the snapshots are failing again.

By setting these environment variables, `.snap.new` files will still be created for any snapshots that change, but mdtest.py will continue as normal anyway so that you can see which assertions are failing.

Reviewing the snapshot changes still has to be done in a separate process to the mdtest.py script. I experimented with having mdtest.py auto-accept all snapshot changes, but invoking `cargo insta accept` every time an mdtest failed made the script much slower.

## Test Plan

I manually tested it by making this change:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 465df24583..37b2f890a4 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -9180,7 +9180,7 @@ impl<'db> IterationError<'db> {
             #[expect(clippy::wrong_self_convention)]
             fn is_not(self, because: impl std::fmt::Display) -> LintDiagnosticGuard<'a, 'a> {
                 let mut diag = self.builder.into_diagnostic(format_args!(
-                    "Object of type `{iterable_type}` is not {maybe_async}iterable",
+                    "Object of type `{iterable_type}` is nott {maybe_async}iterable",
                     iterable_type = self.iterable_type.display(self.db),
                     maybe_async = if self.mode.is_async() { "async-" } else { "" }
                 ));
```

and then incrementally updated failing assertions that were presented to me by mdtest, observing that snapshot failures no longer blocked the script from continuing.

---

_Review requested from @carljm by @AlexWaygood on 2025-10-22 13:58_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-22 13:58_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-22 13:58_

---

_Label `internal` added by @AlexWaygood on 2025-10-22 13:58_

---

_Label `ty` added by @AlexWaygood on 2025-10-22 13:58_

---

_Comment by @github-actions[bot] on 2025-10-22 14:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-22 14:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-10-22 14:29_

Cool — I didn't know these exist. Thank you!

---

_Merged by @AlexWaygood on 2025-10-22 14:32_

---

_Closed by @AlexWaygood on 2025-10-22 14:32_

---

_Branch deleted on 2025-10-22 14:32_

---
