```yaml
number: 19652
title: Fix tests on 32-bit architectures
type: pull_request
state: merged
author: ntBre
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: brent/fix-32bit-tests
created_at: 2025-07-30T22:52:53Z
updated_at: 2025-07-31T12:52:21Z
url: https://github.com/astral-sh/ruff/pull/19652
synced_at: 2026-01-12T15:56:44Z
```

# Fix tests on 32-bit architectures

---

_@ntBre_

Summary
--

Fixes #19640. I'm not sure these are the exact fixes we really want, but I
reproduced the issue in a 32-bit Docker container and tracked down the causes,
so I figured I'd open a PR.

As I commented on the issue, the `goto_references` test depends on the iteration
order of the files in an `FxHashSet` in `Indexed`. In this case, we can just
sort the output in test code.

Similarly, the tuple case depended on the order of overloads inserted in an
`FxHashMap`. `FxIndexMap` seemed like a convenient drop-in replacement, but I
don't know if that will have other detrimental effects. I did have to change the
assertion for the tuple test, but I think it should now be stable across
architectures.

Test Plan
--

Running the tests in the aforementioned Docker container


---

_Comment by @github-actions[bot] on 2025-07-30 22:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @ntBre on 2025-07-30 23:11_

Surprisingly, I don't think the mypy_primer failure is my fault. I tried `main` on pandas-stubs and also got a panic. They're also failing their strict pyright CI check, so I think it's just a problem in the package.

---

_Label `testing` added by @ntBre on 2025-07-30 23:11_

---

_Label `ty` added by @ntBre on 2025-07-30 23:11_

---

_@MichaReiser approved on 2025-07-31 06:15_

Thank you for looking into this!

The goto reference changes look good to me. I'm less sure if this is the right place (it seems unfortunate to pay the overhead of an ordered map to fix a test only issue). I'd appreciate if @AlexWaygood could take a look at that code

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:632 on 2025-07-31 11:31_

tiny nit: I think an [`FxIndexMap`](https://github.com/astral-sh/ruff/blob/27b03a9d7bf779049585bb6cfbf229f18352572d/crates/ty_python_semantic/src/lib.rs#L50) would suffice here, because we shouldn't need to hash it anywhere

---

_@AlexWaygood approved on 2025-07-31 11:32_

This makes sense to me. I doubt that this method will be very hot (attribute access should be cached, and we only enter it for `__getitem__` attribute access on tuple classes), so I don't think switching to an `FxOrderMap` (or `FxIndexMap`) is likely to significantly impact performance here

---

_Comment by @AlexWaygood on 2025-07-31 11:33_

> Surprisingly, I don't think the mypy_primer failure is my fault. I tried `main` on pandas-stubs and also got a panic. They're also failing their strict pyright CI check, so I think it's just a problem in the package.

I merged a workaround for this in https://github.com/astral-sh/ruff/pull/19659 for now

---

_@ntBre reviewed on 2025-07-31 12:34_

---

_Review comment by @ntBre on `crates/ty_python_semantic/src/types/class.rs`:632 on 2025-07-31 12:34_

Ah, makes sense! I had that first and then noticed `FxOrderMap` was already in scope :laughing: I'll switch back

---

_Comment by @github-actions[bot] on 2025-07-31 12:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @ntBre on 2025-07-31 12:44_

---

_Review requested from @carljm by @ntBre on 2025-07-31 12:44_

---

_Review requested from @sharkdp by @ntBre on 2025-07-31 12:44_

---

_Review requested from @dcreager by @ntBre on 2025-07-31 12:44_

---

_Review request for @dcreager removed by @ntBre on 2025-07-31 12:44_

---

_Review request for @carljm removed by @ntBre on 2025-07-31 12:44_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-31 12:44_

---

_@AlexWaygood reviewed on 2025-07-31 12:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:632 on 2025-07-31 12:45_

yeah, I have no idea why `FxOrderMap` is defined in this submodule rather than in `lib.rs` with our other `Fx*` aliases! It should probably be moved

---

_Merged by @ntBre on 2025-07-31 12:52_

---

_Closed by @ntBre on 2025-07-31 12:52_

---

_Branch deleted on 2025-07-31 12:52_

---
