```yaml
number: 21376
title: "[ty] Make `CycleDetector::visit` take `&mut self`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
base: main
head: micha/mut-cycle-detector
created_at: 2025-11-11T09:40:57Z
updated_at: 2025-11-16T11:46:42Z
url: https://github.com/astral-sh/ruff/pull/21376
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Make `CycleDetector::visit` take `&mut self`

---

_Pull request opened by @MichaReiser on 2025-11-11 09:40_

I have a dislike for `RefCell` but I'm not sure if this is worth it. 

Curious to hear what other people think (very happy to close, doesn't have to be a long explanation why)

---

_Label `internal` added by @MichaReiser on 2025-11-11 09:40_

---

_Label `ty` added by @MichaReiser on 2025-11-11 09:40_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 09:42_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 09:44_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Marked ready for review by @MichaReiser on 2025-11-13 18:01_

---

_Review requested from @carljm by @MichaReiser on 2025-11-13 18:01_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-13 18:01_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-13 18:01_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-13 18:01_

---

_Comment by @AlexWaygood on 2025-11-13 18:33_

I think this is basically a straight revert of https://github.com/astral-sh/ruff/pull/19871? I weakly prefer it the way it is just for concision, and I do also sorta agree with Carl's reasoning in https://github.com/astral-sh/ruff/pull/19871#issue-3311732446 :-)

---

_Comment by @carljm on 2025-11-13 18:37_

Yeah, this is just going back to what we originally had. We changed it to use interior mutability because it was a pain to deal with the borrow checker. E.g. it introduces the need for otherwise-unnecessary `collect()` in `tuple.rs`.

I would prefer to leave this as it is.

---

_Closed by @MichaReiser on 2025-11-13 21:53_

---

_Branch deleted on 2025-11-16 11:46_

---
