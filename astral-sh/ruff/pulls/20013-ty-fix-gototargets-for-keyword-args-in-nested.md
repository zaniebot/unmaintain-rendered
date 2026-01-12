```yaml
number: 20013
title: "[ty] fix GotoTargets for keyword args in nested function calls"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/ref
created_at: 2025-08-20T22:56:02Z
updated_at: 2025-08-21T20:19:53Z
url: https://github.com/astral-sh/ruff/pull/20013
synced_at: 2026-01-12T15:56:52Z
```

# [ty] fix GotoTargets for keyword args in nested function calls

---

_@Gankra_

While implementing similar logic for initializers I noticed that this code appeared to be walking the ancestors in the wrong direction, and so if you have nested function calls it would always grab the outermost one instead of the closest-ancestor.

The four copies of the test are because there's something really evil in our caching that can't seem to be demonstrated in our cursor testing framework, which I'm filing a followup for.

---

_Label `server` added by @Gankra on 2025-08-20 22:56_

---

_Review requested from @carljm by @Gankra on 2025-08-20 22:56_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-20 22:56_

---

_Label `ty` added by @Gankra on 2025-08-20 22:56_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-20 22:56_

---

_Review requested from @sharkdp by @Gankra on 2025-08-20 22:56_

---

_Review requested from @dcreager by @Gankra on 2025-08-20 22:56_

---

_Comment by @github-actions[bot] on 2025-08-20 22:57_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-20 23:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @carljm removed by @carljm on 2025-08-20 23:11_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:602 on 2025-08-21 06:21_

I think we should fix this in `ancestors` instead. At least one of the remaining references are equally wrong and we can call `rev` for the remaining usages where they want to walk the ancestors top to bottom (but bottom up is more intuitive to me)

---

_@MichaReiser approved on 2025-08-21 06:21_

---

_@Gankra reviewed on 2025-08-21 14:58_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:602 on 2025-08-21 14:58_

Yeah good call, it does seem like a footgun

---

_Merged by @Gankra on 2025-08-21 20:19_

---

_Closed by @Gankra on 2025-08-21 20:19_

---

_Branch deleted on 2025-08-21 20:19_

---
