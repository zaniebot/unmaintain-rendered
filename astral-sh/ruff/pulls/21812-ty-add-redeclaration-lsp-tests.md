```yaml
number: 21812
title: "[ty] Add redeclaration LSP tests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/redeclaration-tests
created_at: 2025-12-05T13:10:44Z
updated_at: 2025-12-05T18:02:35Z
url: https://github.com/astral-sh/ruff/pull/21812
synced_at: 2026-01-12T15:57:34Z
```

# [ty] Add redeclaration LSP tests

---

_@MichaReiser_

## Summary

Add tests that demonstrate how ty handles redeclarations:

```py
a: str = "test"

a: int = 10

print(a<CURSOR>)

a: bool = True
```

Curious to here if my expectations align with everyon else's. 

Basically, my thinking is that `a: int` introduces a new declaration and features like:

* find references
* go to definition
* doc highlighting
* rename
* ...


should all ignore the `a: str` declaration, because it's a separate declaration.

## Test Plan

Added tests


---

_Label `testing` added by @MichaReiser on 2025-12-05 13:10_

---

_Review requested from @carljm by @MichaReiser on 2025-12-05 13:10_

---

_Label `ty` added by @MichaReiser on 2025-12-05 13:10_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-05 13:10_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-05 13:10_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-05 13:10_

---

_Label `testing` added by @MichaReiser on 2025-12-05 13:10_

---

_Label `ty` added by @MichaReiser on 2025-12-05 13:10_

---

_Label `server` added by @AlexWaygood on 2025-12-05 13:11_

---

_Comment by @astral-sh-bot[bot] on 2025-12-05 13:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-05 13:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/rename.rs`:1434 on 2025-12-05 17:48_

note that in `main.py` on line 1451/1450, we still import `test` from `lib2` (a module that doesn't exist), not `lib`. I don't know if that's intended or not. (I deliberately preserved that behaviour in https://github.com/astral-sh/ruff/pull/21810, because I wasn't sure if that was intended or not 😄)

---

_@AlexWaygood approved on 2025-12-05 17:52_

I agree with all of these!

---

_@MichaReiser reviewed on 2025-12-05 17:54_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/rename.rs`:1434 on 2025-12-05 17:54_

Lol, it was not and it seems I messed it up again when merging the changes

---

_Merged by @MichaReiser on 2025-12-05 18:02_

---

_Closed by @MichaReiser on 2025-12-05 18:02_

---

_Branch deleted on 2025-12-05 18:02_

---
