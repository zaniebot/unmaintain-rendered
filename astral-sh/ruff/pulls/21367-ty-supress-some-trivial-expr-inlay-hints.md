```yaml
number: 21367
title: "[ty] supress some trivial expr inlay hints"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/ctor-hints
created_at: 2025-11-10T19:12:13Z
updated_at: 2025-11-10T19:51:15Z
url: https://github.com/astral-sh/ruff/pull/21367
synced_at: 2026-01-12T15:57:21Z
```

# [ty] supress some trivial expr inlay hints

---

_@Gankra_

I'm not 100% sold on this implementation, but it's a strict improvement and it adds a ton of snapshot tests for future iteration.

Part of https://github.com/astral-sh/ty/issues/494

---

_Review requested from @carljm by @Gankra on 2025-11-10 19:12_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-10 19:12_

---

_Review requested from @sharkdp by @Gankra on 2025-11-10 19:12_

---

_Review requested from @dcreager by @Gankra on 2025-11-10 19:12_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-10 19:12_

---

_Label `server` added by @Gankra on 2025-11-10 19:12_

---

_Label `ty` added by @Gankra on 2025-11-10 19:12_

---

_@Gankra reviewed on 2025-11-10 19:13_

---

_Review comment by @Gankra on `crates/ty_ide/src/inlay_hints.rs`:461 on 2025-11-10 19:13_

This case is why this approach is mediocre. The type-directed approach would catch this trivially.

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 19:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-10 19:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/inlay_hints.rs`:461 on 2025-11-10 19:17_

I don't think this is actually so bad? It tells the user that we inferred the type of `y` as `Literal[1]`, but mypy would infer it as `int` here -- that's potentially useful information. The `y = x` assignment could also happen much lower down in the function, far away from the initial `x = 1` assignment

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/inlay_hints.rs`:490 on 2025-11-10 19:19_

this, on the other hand, feels kinda yucky to me 😆

---

_@AlexWaygood approved on 2025-11-10 19:19_

---

_Merged by @Gankra on 2025-11-10 19:51_

---

_Closed by @Gankra on 2025-11-10 19:51_

---

_Branch deleted on 2025-11-10 19:51_

---
