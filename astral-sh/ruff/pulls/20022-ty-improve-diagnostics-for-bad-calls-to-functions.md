```yaml
number: 20022
title: "[ty] Improve diagnostics for bad calls to functions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: unknown-argument-show-def
created_at: 2025-08-21T13:09:22Z
updated_at: 2025-08-21T21:00:45Z
url: https://github.com/astral-sh/ruff/pull/20022
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Improve diagnostics for bad calls to functions

---

_Pull request opened by @AlexWaygood on 2025-08-21 13:09_

## Summary

It was annoyingly hard to debug https://github.com/astral-sh/ty/issues/1076. It wasn't clear whether ty was resolving the type definition to a location in pandas or pandas-stubs; and it wasn't clear what ty thought the signature of the function _was_ (the diagnostic only said that the user had got the signature wrong).

This PR adds subdiagnostics to our `missing-argument`, `too-many-positional-arguments` and `unknown-argument` error codes, so that these will be easier to debug in the future -- both for users, and for us!

I've taken care not to add the subdiagnostics if it's a union of callables being called. We currently emit a separate diagnostic for each element of the union; having a sub-diagnostic for each element would probably be too verbose for it to be worth it, in my opinion.

## Test Plan

Snapshots.


---

_Review requested from @carljm by @AlexWaygood on 2025-08-21 13:09_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-21 13:09_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-21 13:09_

---

_Label `ty` added by @AlexWaygood on 2025-08-21 13:09_

---

_Label `diagnostics` added by @AlexWaygood on 2025-08-21 13:09_

---

_Comment by @github-actions[bot] on 2025-08-21 13:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-21 13:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-21 13:45_

Not sure why GitHub is repeatedly failing to checkout this branch on Windows, but it doesn't seem related to the changes being made here

---

_Comment by @AlexWaygood on 2025-08-21 20:29_

oh I put a Bad Windows Character in a generated snapshot path, didn't I?

---

_@carljm approved on 2025-08-21 20:58_

Nice!

Not sure I agree about unions -- in every case shown in these snapshot tests (and I think probably in the majority of real-world cases, too), there's still just one diagnostic because only one element of the union failed the call, and the union context is just an additional info line or two. In all of those cases, I think the extra subdiagnostic (regarding the one union element that failed the call) would be an improvement and useful in debugging, it wouldn't be too much info. It's possible that there are cases where many union elements fail the call; those might be cases where it makes sense to skip the sub-diagnostic?

But it looks like that would be an additional feature here, not just removing a restriction; I don't think we need to spend more time on it now.

---

_Merged by @AlexWaygood on 2025-08-21 21:00_

---

_Closed by @AlexWaygood on 2025-08-21 21:00_

---

_Branch deleted on 2025-08-21 21:00_

---
