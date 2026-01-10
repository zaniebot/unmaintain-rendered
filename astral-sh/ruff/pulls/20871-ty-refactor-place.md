```yaml
number: 20871
title: "[ty] refactor `Place`"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: refactor-place
created_at: 2025-10-14T19:25:28Z
updated_at: 2025-10-16T00:14:57Z
url: https://github.com/astral-sh/ruff/pull/20871
synced_at: 2026-01-10T17:34:34Z
```

# [ty] refactor `Place`

---

_Pull request opened by @mtshiba on 2025-10-14 19:25_

## Summary

Part of astral-sh/ty#1341

The following changes will be made to `Place`.

* Introduce `TypeOrigin`
* `Place::Type` -> `Place::Defined`
* `Place::Unbound` -> `Place::Undefined`
* `Boundness` -> `Definedness`

`TypeOrigin::Declared`+`Definedness::PossiblyUndefined` are patterns that weren't considered before, but this PR doesn't address them yet, only refactors.

## Test Plan

N/A


---

_Comment by @github-actions[bot] on 2025-10-14 19:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-14 19:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @mtshiba on 2025-10-14 19:52_

Hmm, the diff in mypy_primer is unexpected.
There was no diff in the initial commit, and it appears that the diff was caused by a merge commit that should have been unrelated.

It seems that this also occurred in #20792 and #20806.
I don't know the cause, but perhaps there is non-deterministic inference being made regarding `dataclasses.field` or overloading?

---

_Marked ready for review by @mtshiba on 2025-10-14 19:54_

---

_Review requested from @carljm by @mtshiba on 2025-10-14 19:54_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-10-14 19:54_

---

_Review requested from @sharkdp by @mtshiba on 2025-10-14 19:54_

---

_Review requested from @dcreager by @mtshiba on 2025-10-14 19:54_

---

_Label `ty` added by @amyreese on 2025-10-14 21:46_

---

_Comment by @AlexWaygood on 2025-10-14 21:52_

> but perhaps there is non-deterministic inference being made regarding `dataclasses.field` or overloading?

Yeah, there does seem to be something nondeterministic happening there. It's quite annoying; if you feel like tracking it down, that would be very helpful 😄

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/member.rs`:8 on 2025-10-15 11:11_

I think I'm inclined to keep `Member` as a struct, even if it only consists of `inner: pub(super) inner: PlaceAndQualifiers<'db>` for now. I am quite confident that we will add other attributes to this soon.

But great to see that the new `Place` makes the `is_declared` flag superfluous.

---

_@sharkdp approved on 2025-10-15 11:13_

Thank you for doing this! Just one comment.

If someone is unhappy with the newly proposed names here, we can always easily change them later. I think we should aim to get this refactor in as quick as possible, or it will collect a lot of conflicts.

---

_@dcreager approved on 2025-10-15 12:52_

Thank you for tackling this! I agree with David that we should get this in sooner rather than later, and tackle any proposed changes as follow ons. This looks good enough to land to me!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-15 12:55_

---

_Review requested from @sharkdp by @mtshiba on 2025-10-15 17:32_

---

_Merged by @sharkdp on 2025-10-15 18:19_

---

_Closed by @sharkdp on 2025-10-15 18:19_

---

_Comment by @sharkdp on 2025-10-15 18:19_

Thank you!

---

_Branch deleted on 2025-10-16 00:14_

---
