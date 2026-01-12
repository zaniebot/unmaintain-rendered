```yaml
number: 20865
title: "[ty] Add (unused) `inferable` parameter to type property methods"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/non-inferable-api
created_at: 2025-10-14T14:08:45Z
updated_at: 2025-10-15T13:05:18Z
url: https://github.com/astral-sh/ruff/pull/20865
synced_at: 2026-01-12T15:57:11Z
```

# [ty] Add (unused) `inferable` parameter to type property methods

---

_@dcreager_

A large part of the diff on #20677 just involves threading a new `inferable` parameter through all of the type property methods. In the interests of making that PR easier to review, I've pulled that bit out into here, so that it can be reviewed in isolation. This should be a pure refactoring, with no logic changes or behavioral changes.

---

_Review requested from @carljm by @dcreager on 2025-10-14 14:08_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-14 14:08_

---

_Review requested from @sharkdp by @dcreager on 2025-10-14 14:08_

---

_Label `internal` added by @dcreager on 2025-10-14 14:08_

---

_Label `ty` added by @dcreager on 2025-10-14 14:08_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-14 14:08_

---

_Comment by @github-actions[bot] on 2025-10-14 14:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-14 14:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-14 14:14_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://dcreager-non-inferable-api.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-non-inferable-api.ecosystem-663.pages.dev/timing))


---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/generics.rs`:151 on 2025-10-15 11:01_

:+1:

I used the same trick in a refactoring yesterday :smile: 

---

_@sharkdp approved on 2025-10-15 11:03_

Thank you for pulling this out.

---

_@MichaReiser reviewed on 2025-10-15 12:30_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/generics.rs`:151 on 2025-10-15 12:30_

What's the benefit of using `PhantomData` over `()`? Ah, never mind, we need the lifetime

---

_@dcreager reviewed on 2025-10-15 12:59_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:151 on 2025-10-15 12:59_

> I used the same trick in a refactoring yesterday

Blast, no fist-bump reaction emoji here! Please know that I am giving you a fist bump.

---

_Merged by @dcreager on 2025-10-15 13:05_

---

_Closed by @dcreager on 2025-10-15 13:05_

---

_Branch deleted on 2025-10-15 13:05_

---
