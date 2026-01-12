```yaml
number: 17161
title: "[red-knot] Fix inference for `pow` between two literal integers"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/pow-pow
created_at: 2025-04-02T21:21:25Z
updated_at: 2025-04-03T06:19:17Z
url: https://github.com/astral-sh/ruff/pull/17161
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Fix inference for `pow` between two literal integers

---

_@AlexWaygood_

## Summary

Python `**` works differently to Rust `**`!

## Test Plan

Added an mdtest for various edge cases, and checked in the Python REPL that we infer the correct type in all the new cases tested.


---

_Label `bug` added by @AlexWaygood on 2025-04-02 21:21_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-02 21:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-02 21:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-02 21:21_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-02 21:21_

---

_@carljm approved on 2025-04-02 21:24_

Thank you!

---

_Comment by @github-actions[bot] on 2025-04-02 21:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:4620 on 2025-04-02 21:25_

Would we fall back to `float` anyway via typeshed stubs if we guard the whole match pattern with `m < 0`? Or do we not understand the typeshed stubs yet? Either way, I'm also fine with hardcoding `float` here.

---

_@sharkdp approved on 2025-04-02 21:25_

Thank you

---

_Merged by @AlexWaygood on 2025-04-02 21:25_

---

_Closed by @AlexWaygood on 2025-04-02 21:25_

---

_Branch deleted on 2025-04-02 21:25_

---

_@AlexWaygood reviewed on 2025-04-02 21:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4620 on 2025-04-02 21:30_

oops, merged before I saw this. Yes, that would also work! But, eh, this is probably a tiny bit more performant than looking up the stub in typeshed, and it's unlikely to ever change at runtime ;)

---

_@MichaReiser reviewed on 2025-04-03 06:19_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:4620 on 2025-04-03 06:19_

I doubt there are many code bases that have so many pow operations that the performance difference is noticeable 😉

---
