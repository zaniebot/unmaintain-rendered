```yaml
number: 21732
title: "[ty] Fix find references for type defined in stub"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-references-for-type-defined-in-stub
created_at: 2025-12-01T15:04:13Z
updated_at: 2025-12-01T16:53:47Z
url: https://github.com/astral-sh/ruff/pull/21732
synced_at: 2026-01-12T15:57:32Z
```

# [ty] Fix find references for type defined in stub

---

_@MichaReiser_

## Summary

ty failed to return any references for a type defined in a stub where
there's also a definition in a non-stub file.

The underlying issue was that `find_references` resolved the name to
"go-to definition" whereas the visitor resolved the declaration, which
can resolve to a different file and location.

This fix uses go to declaration in both steps. I don't think it really
matters whether we use go to declaration or go to definition because
both should resolve to the same references. I picked go to declaration
because it requires less work (no stub mapping)

## Test Plan

Added test


---

_Label `server` added by @MichaReiser on 2025-12-01 15:04_

---

_Label `ty` added by @MichaReiser on 2025-12-01 15:04_

---

_Review requested from @carljm by @MichaReiser on 2025-12-01 15:04_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-01 15:04_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-01 15:04_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-01 15:04_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 15:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-01 15:06_

---

_Review requested from @Gankra by @MichaReiser on 2025-12-01 15:07_

---

_Review request for @carljm removed by @MichaReiser on 2025-12-01 15:07_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-01 15:07_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-01 15:07_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 15:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @Gankra on 2025-12-01 15:10_

Hmm interesting, ideally we would resolve both..? Especially if you're in a codebase that maintains the `.py` and `.pyi`. I wonder what different LSPs do...

---

_Comment by @MichaReiser on 2025-12-01 15:13_

> Hmm interesting, ideally we would resolve both..? Especially if you're in a codebase that maintains the `.py` and `.pyi`. I wonder what different LSPs do...

We only use the first navigation target to test if they're the same. 

As far as I could tell, ty now shows the same references when finding references to `Path` in `ty_benchmark` as Pylance

---

_@Gankra approved on 2025-12-01 15:17_

Certainly this makes more sense than what's currently there.

---

_Comment by @MichaReiser on 2025-12-01 15:19_

I'm certainly not saying this is the final solution. It's just one of the first issues I found while trying to wrap my head around how find references works

---

_Merged by @MichaReiser on 2025-12-01 16:53_

---

_Closed by @MichaReiser on 2025-12-01 16:53_

---

_Branch deleted on 2025-12-01 16:53_

---
