```yaml
number: 16124
title: "[red-knot] Outline integration point for `len` diagnostics"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: micha/call-outcome-step1
head: micha/call-outcome-step2
created_at: 2025-02-12T17:20:32Z
updated_at: 2025-02-13T08:04:35Z
url: https://github.com/astral-sh/ruff/pull/16124
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Outline integration point for `len` diagnostics

---

_Pull request opened by @MichaReiser on 2025-02-12 17:20_

## Summary

This step continues with what the first PR started by moving out type checking operations from the `Type::` operations. 

I found `len` an interesting use case because it may lead to a fair amount of code duplication. 

I'm still not a 100% convinced by this approach because it can require performing some operations twice:

1. When infering the return type of the call
2. When checking the call itself

We could avoid some of that by adding more information to the `Outcome` but I'm not too much a fan of that 
because it means larger return types for everyone. 

## Test Plan



---

_@MichaReiser reviewed on 2025-02-12 17:21_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3349 on 2025-02-12 17:21_

 This probably requires calling `__len__` to get the `CallDunderLenOutcome` and then verify the `binding` on it.

---

_Label `red-knot` added by @AlexWaygood on 2025-02-12 17:22_

---

_@MichaReiser reviewed on 2025-02-12 17:25_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1910 on 2025-02-12 17:25_

What's slightly confusing here is that `CallOutcome` also has a `PossiblyUnbound` variant which can also be in the `Call` variant. I feel like we should only have one?

---

_Comment by @MichaReiser on 2025-02-13 08:04_

Okay, this won't work because `len` won't be checked anymore if we call a union:

```py
class NotSized: ...

def test(cond: bool):
	if cond:
		a = len
	else:
		a = |x| 1

	a(NotSized) # should error
```


This makes me think that we probably want to move call validation into the `Binder` which already emits similar errors for regular methods.

---

_Closed by @MichaReiser on 2025-02-13 08:04_

---
