```yaml
number: 21587
title: "[ty] Refactor `CheckSuppressionContext` to use `DiagnosticGuard`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/suppressions-diagnostic-guard
created_at: 2025-11-23T10:25:01Z
updated_at: 2025-11-25T10:54:44Z
url: https://github.com/astral-sh/ruff/pull/21587
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Refactor `CheckSuppressionContext` to use `DiagnosticGuard`

---

_@MichaReiser_

## Summary

This is PR is a first step towards adding a "Remove unused suppression" code action in the editor. 

Adding the fix for removing unused suppression requires setting the `Fix` on the 
diagnostic created by `CheckSuppressionContext::report_unchecked`. To make this possible,
change `report_unchecked` and `report_lint` to return a `DiagnosticGuard`.

Doing so requires extracting `DiagnosticGuard` from `InferContext`. 

I tried very hard to not having to change `CheckSuppressionContext` to store a `RefCell<TypeCheckDiagnostic>` but
I couldn't find a way to do so. I tried a `enum DiagnosticSink { Cell(RefCell<TypeCheckDiagnostics>), Mut(&mut TypeCheckDiagnostic) }` 
with a `emit` method that takes `&self`. However, the borrow checker (correctly?), disallowes me to mutate the `&mut TypeCheckDiagnostic` in that case. 

I gave up after 30min or so and I sort of like how `check_suppressions` takes the `Diagnostic`s with suppressions and returns only
the diagnostics once it's done, as the suppressions have now been handled.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-11-23 10:25_

---

_Label `ty` added by @MichaReiser on 2025-11-23 10:25_

---

_@MichaReiser reviewed on 2025-11-23 10:25_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/diagnostic.rs`:154 on 2025-11-23 10:25_

This is just a move except that `DiagnosticGuard` now stores a `file` and `sink` instead of a `&InferContext`

---

_Comment by @astral-sh-bot[bot] on 2025-11-23 10:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-23 10:28_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-11-23 10:51_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/context.rs`:636 on 2025-11-23 10:51_

I moved this logic into `DiagnosticGuard::drop`, to reduce the code duplication

---

_Review requested from @BurntSushi by @MichaReiser on 2025-11-23 10:51_

---

_Renamed from "[ty] Refactor `CheckSuppressionContext` to use `DiagnosticGuard`"" to "[ty] Refactor `CheckSuppressionContext` to use `DiagnosticGuard`" by @MichaReiser on 2025-11-23 11:03_

---

_Marked ready for review by @MichaReiser on 2025-11-23 11:13_

---

_Review requested from @carljm by @MichaReiser on 2025-11-23 11:13_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-23 11:13_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-23 11:13_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-23 11:13_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-23 11:13_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-23 11:13_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-23 11:13_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-23 11:13_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/diagnostic.rs`:168 on 2025-11-24 15:02_

What goes wrong when you use `&mut TypeCheckDiagnostics` here?

It might be worth a brief comment saying why interior mutability is being used here.

---

_@BurntSushi approved on 2025-11-24 15:02_

Makes sense to me!

---

_@MichaReiser reviewed on 2025-11-24 15:11_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/diagnostic.rs`:168 on 2025-11-24 15:11_

It would panic if you do:

```
let diag = context.report_lint(...);
let diag2 = context.report_lint(...); // we now borrow `&mut TypeCheckDiagnostics` twice
```

This seems *worse* as an API caller shouldn't be aware of the internal ref-cell borrow 

---

_@BurntSushi reviewed on 2025-11-24 15:26_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/diagnostic.rs`:168 on 2025-11-24 15:26_

I guess I meant the higher level reason. But I think I now see: you wouldn't be able to have two guards alive at the same time?

---

_@MichaReiser reviewed on 2025-11-24 17:52_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/diagnostic.rs`:168 on 2025-11-24 17:52_

Yes, the higher level reason is that the current approach correctly isolates the `RefCell` so that callers don't need to know about it (I'll add a comment). 

---

_Merged by @MichaReiser on 2025-11-25 10:54_

---

_Closed by @MichaReiser on 2025-11-25 10:54_

---

_Branch deleted on 2025-11-25 10:54_

---
