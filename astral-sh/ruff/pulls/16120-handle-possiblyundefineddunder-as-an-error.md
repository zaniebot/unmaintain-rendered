```yaml
number: 16120
title: "Handle `PossiblyUndefinedDunder` as an error"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
base: main
head: micha/dunder-call-possibly-unbound
created_at: 2025-02-12T14:48:16Z
updated_at: 2025-02-12T16:27:48Z
url: https://github.com/astral-sh/ruff/pull/16120
synced_at: 2026-01-12T15:55:53Z
```

# Handle `PossiblyUndefinedDunder` as an error

---

_@MichaReiser_

## Summary

I won't pretend that I know what I'm doing here. 

However, I noticed that our handling of `PossiblyUnbound` for dunder methods is inconsistent:

* `CallDunderResult::return_type` returns `None` 
* `CallOutcome::return_type` might return `Some` for `PossiblyUnboundDunder` unlike all other variants taht return an `Err` in `return_type_result`.


To me, this seems wrong. The idea of those methods is to return the type if it is know but the possibly unbound suggests that the type is actually not known. 
Unfortunately, there are no tests... 


Edit: I included a small refactor in this PR that renames `CallDunderResult` to `CallDunderOutcome`. But it's a separate commit to ease reviewing.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-02-12 14:48_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-12 14:48_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-12 14:48_

---

_Label `red-knot` added by @MichaReiser on 2025-02-12 14:48_

---

_@carljm reviewed on 2025-02-12 15:53_

I don't see an inconsistency here, or anything that needs fixing?

There are many cases where we want to raise a diagnostic about something, but we still want to return a type. In this case, the dunder might be unbound, which is an error, but if it is bound, we know what its type must be, and the best thing for the user is to a) emit a diagnostic about it being possibly unbound, but also b) check the rest of the code with that type, rather than with `Unknown`.

Whatever refactoring we do with operations of type outcomes, it cannot rely on an assumption that "emit a diagnostic" and "return a type" are mutually exclusive.

Edit: on re-reading the summary, I understand where it looks like an inconsistency, because `return_type_result` has to choose between `Err` and `Ok`. But even the `Err` variant it returns still includes the actual known type, which matches what is returned from `return_type`. The core problem here is that the contract of `return_type_result` is too limiting, because `Ok` and `Err` are the only options, so there isn't a clear way to express "we need to emit a diagnostic, but we also do have a type from this operation." (I think this issue with `return_type_result` is at the core of a lot of the API problems on `CallOutcome`.) But I think the behavior of `return_type` (given its contract "give me the type, I don't care about diagnostics") is fine here, if anything it is `return_type_result` that is problematic (but its current API contract doesn't give it any better options.)

---

_Comment by @MichaReiser on 2025-02-12 16:27_

I see. That makes me lean even stronger into separating checking vs this public: try guess a type API ;) 



---

_Closed by @MichaReiser on 2025-02-12 16:27_

---
