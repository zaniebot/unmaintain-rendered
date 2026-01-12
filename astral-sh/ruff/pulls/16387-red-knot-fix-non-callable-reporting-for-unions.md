```yaml
number: 16387
title: "[red-knot] fix non-callable reporting for unions"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/unioncall
created_at: 2025-02-26T00:48:54Z
updated_at: 2025-02-26T15:06:06Z
url: https://github.com/astral-sh/ruff/pull/16387
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] fix non-callable reporting for unions

---

_@carljm_

Minor follow-up to https://github.com/astral-sh/ruff/pull/16161

This `not_callable` flag wasn't functional, because it could never be `false`. It was initialized to `true` and then only ever updated with `|=`, which can never make it `false`.

Add a test that exercises the case where it _should_ be `false` (all of the union elements are callable) but `bindings` is also empty (all union elements have binding errors). Before this PR, the added test wrongly emits a diagnostic that the union `Literal[f1] | Literal[f2]` is not callable.

And add a test where a union call results in one binding error and one not-callable error, where we currently give the wrong result (we show only the binding error), with a TODO.

Also add TODO comments in a couple other tests where ideally we'd report more than just one error out of a union call.

Also update the flag name to `all_errors_not_callable` to more clearly indicate the semantics of the flag.


---

_Review requested from @MichaReiser by @carljm on 2025-02-26 00:48_

---

_Review requested from @AlexWaygood by @carljm on 2025-02-26 00:48_

---

_Review requested from @sharkdp by @carljm on 2025-02-26 00:48_

---

_@dhruvmanila approved on 2025-02-26 03:36_

Nice catch!

---

_Label `red-knot` added by @dhruvmanila on 2025-02-26 03:36_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:38 on 2025-02-26 07:22_

I think this is now wrong but in different ways then before? 

If I understand this correctly, it will now return `not_callable` if one variant is not callable and the other has binding errors. What I think we want is to only return `not_callable` if all errors are not_callable. 

So the fix might be to keep initializing the variable to `true` but change the error case to `not_callable &= error.is_not_callable()`

---

_@MichaReiser reviewed on 2025-02-26 07:22_

---

_@carljm reviewed on 2025-02-26 13:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:38 on 2025-02-26 13:19_

I don't think that behavior would be correct. A union with at least one not-callable element is not callable.

In general for any type it is always true that some inhabitants of the type may permit some behavior that the overall type does not permit. The definition of a type supporting an operation is that all inhabitants support it.

I would like us to offer more detailed diagnostics for not-callable unions, where we would clarify which member(s) are not callable. In that world, I do think we might want to return `CallError::Union` in more cases. But with the behavior today, where `CallError::Union` only reports one of its errors, I think it would be wrong if we ignored a not-callable error from one element and just reported a binding error from some other type in the union. So I think we need to return NotCallable if any element is not callable. 

---

_@MichaReiser reviewed on 2025-02-26 13:58_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:38 on 2025-02-26 13:58_

I suspect that this will lead to worse diagnostics overall because we then end up saying that the type isn't callable instead of saying which variant isn't callable. 

I can't argue on what this means on the type inference level but your original implementation only returned `NotCallable` if all variants weren't callable, unless I'm misreading the implementation:

https://github.com/astral-sh/ruff/blob/792f9e357ee21a23113aba75e36c8e1ca47fa386/crates/red_knot_python_semantic/src/types/call.rs#L240-L282



---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:38 on 2025-02-26 14:22_

> your original implementation only returned `NotCallable` if all variants weren't callable, unless I'm misreading the implementation

Both branches in the linked original implementation return `NotCallableError`, whether some or all elements are not callable. The only difference is whether we return a "simple" `NotCallableError` (because if all elements are not callable, we don't really need to give more details) or a "union" `NotCallableError` (because we want to clarify which elements are not callable.)

> we then end up saying that the type isn't callable instead of saying which variant isn't callable.

I agree. The core problem here is the current handling of `CallError::Union` only reporting one of its errors. This means that in a case where we have one binding error and one not-callable in a union, might decide to just report the binding error and ignore the not-callable error. That's the wrong result I aimed to avoid with this change. In that case I would much rather see a not-callable error on the entire union, than a wrong-arguments error for the one callable element.

We can go ahead and return `CallError::Union` for that case. It will just increase the priority of better diagnostics for `CallError::Union`.

---

_@carljm reviewed on 2025-02-26 14:22_

---

_@MichaReiser reviewed on 2025-02-26 14:27_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:38 on 2025-02-26 14:27_

I'd prefer keeping returning `Union` because it already forces the right handling in all upstream code. 

Fixing the diagnostic is an issue on its own where we may want to be more clever about how we handle different errors (I don't think rendering all errors is a good idea but we could sort the errors and e.g. report not callable first)

---

_Comment by @AlexWaygood on 2025-02-26 14:30_

I'm not sure whether it counts as an alternative solution or a workaround, but did you see what I did with this in my `Type::try_iterate()` PR the other day? I added this method:

https://github.com/astral-sh/ruff/blob/dd6f6233bd469ddc9296c1ebd7cdb845233c93b1/crates/red_knot_python_semantic/src/types/call.rs#L175-L195

And then emitted different diagnostics for the `CallError::Union()` branch depending on whether it returned `true` or `false`:

https://github.com/astral-sh/ruff/blob/dd6f6233bd469ddc9296c1ebd7cdb845233c93b1/crates/red_knot_python_semantic/src/types.rs#L3032-L3071

It's finicky, but it seems to work pretty well

---

_Comment by @carljm on 2025-02-26 14:34_

@AlexWaygood Yes, we could do that here as well. But I think it's also sub-optimal that given multiple binding errors from a union, we only report one of them. So I think I'd rather just wait for a more full-fledged diagnostic system that allows sub-diagnostics, and then update the `CallError::Union` handling more comprehensively.

---

_@MichaReiser approved on 2025-02-26 14:43_

Thank you

---

_Merged by @carljm on 2025-02-26 15:06_

---

_Closed by @carljm on 2025-02-26 15:06_

---

_Branch deleted on 2025-02-26 15:06_

---
