```yaml
number: 19999
title: "[ty] Avoid unnecessary argument type expansion"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: dhruv/avoid-argument-type-expansion
created_at: 2025-08-20T06:29:08Z
updated_at: 2025-08-21T06:13:12Z
url: https://github.com/astral-sh/ruff/pull/19999
synced_at: 2026-01-12T15:56:52Z
```

# [ty] Avoid unnecessary argument type expansion

---

_@dhruvmanila_

## Summary

Part of: https://github.com/astral-sh/ty/issues/868

This PR adds a heuristic to avoid argument type expansion if it's going to eventually lead to no matching overload.

This is done by checking whether the non-expandable argument types are assignable to the corresponding annotated parameter type. If one of them is not assignable to all of the remaining overloads, then argument type expansion isn't going to help.

## Test Plan

Add mdtest that would otherwise take a long time because of the number of arguments that it would need to expand (30).


---

_Label `performance` added by @dhruvmanila on 2025-08-20 06:29_

---

_Label `ty` added by @dhruvmanila on 2025-08-20 06:29_

---

_Comment by @github-actions[bot] on 2025-08-20 06:31_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-20 06:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dhruvmanila reviewed on 2025-08-20 10:02_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:679 on 2025-08-20 10:02_

Considering the same example, but pass in `A()` as the first argument and use `a=None` instead of `a=1` for the parameter above, this would still continue with the expansion. The reason being that `A()` matches parameter of at least one overload which means we cannot skip argument type expansion.

I'm trying to think through what kind of heuristic to have that would avoid the expansion in this case as well. This would still lead to no-matching-overload because even though `Unknown` is assignable to `int`, the spec says that _all_ argument lists resulting from an expansion should evaluate successfully and there's no such expansion as expanding `Unknown | None` leads to argument lists where at least one would have `None` which isn't assignable to `int`.

Regardless, I think it'd be best to set a higher limit on the expansion.

---

_Comment by @dhruvmanila on 2025-08-20 10:17_

EDIT: Nevermind, it _is_ because ty doesn't support `**kwargs` and the new logic should avoid checking those arguments to skip argument type expansion for now.

> ```diff
> rotki (https://github.com/rotki/rotki)
> + rotkehlchen/tests/db/test_db.py:377:13: error[no-matching-overload] No overload of bound method `set_dynamic_cache` matches arguments
> + rotkehlchen/tests/db/test_db.py:378:20: error[no-matching-overload] No overload of bound method `get_dynamic_cache` matches arguments
> + rotkehlchen/tests/db/test_db.py:418:20: error[no-matching-overload] No overload of bound method `get_dynamic_cache` matches arguments
> - Found 1588 diagnostics
> + Found 1591 diagnostics
> ```

I wasn't expecting any mypy-primer diff but it seems that we have new diagnostics :)

I think this might be due to the fact that ty doesn't support `**kwargs` yet but I'm looking into it a bit closely. The main reason I think that is because when I remove the `**kwargs` in the call, the diagnostic disappear.

https://github.com/user-attachments/assets/6cecc561-3252-468d-8d4b-551d7f537268



---

_Marked ready for review by @dhruvmanila on 2025-08-20 10:17_

---

_Review requested from @carljm by @dhruvmanila on 2025-08-20 10:17_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-08-20 10:17_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-20 10:17_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-20 10:17_

---

_@MichaReiser reviewed on 2025-08-20 17:38_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/arguments.rs`:230 on 2025-08-20 17:38_

Should we add a note to `expand_type` to update `is_type_expandable` (maybe rename to `is_expandable_type` for symmetry)  if necessary

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:679 on 2025-08-20 18:26_

Yes, I think there will be cases where we can't avoid actually trying all the expansions and see if any of them work -- best we can do there is optimize the expansions as best we can (e.g. iterator instead of eagerly materialized?) and then set a reasonable limit.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1331 on 2025-08-20 18:30_

Would it make sense to first explicitly check if there are any expandable arguments (using the new method you added), then do this check, then actually expand the arguments? This way even if the heuristic applies, we've already generated the expansion, just haven't used the expansions yet.

But maybe generating the expansions is very cheap relative to actually trying the call with new argument types, so this doesn't matter?

And I guess we'd slow down the fast path slightly if we first check all arguments for expandability, then separately expand them?

---

_@carljm approved on 2025-08-20 18:30_

Nice, thank you!

---

_@dhruvmanila reviewed on 2025-08-21 05:59_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1331 on 2025-08-21 05:59_

Yeah, I don't think this would really matter in practice mainly because there are only a few differences between `expand_type` and `is_type_expandable` which are (1) collecting the enum members from the metadata (both method generates the metadata) (2) performing multi-cartesian product of tuple elements and (3) allocation. I guess it wouldn't hurt to avoid allocating when it's easy to do so. Let me try it, I think I'll make this change.

---

_@dhruvmanila reviewed on 2025-08-21 06:06_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1331 on 2025-08-21 06:06_

Ah right, so the main reason I did it at this location is so that the bindings state is the one before the type checking step, so that we only skip the overloads that have been filtered by the arity check.

So, we'd either have to pay the cost of either
- Allocation for the first expansion, taking the bindings snapshot. We'd skip the snapshot if there are no expansion in this case.
- Always take the binding snapshot, check whether expansion needs to happen using the logic in this PR, skip allocating for the first expansion if there's no need

I think going with the latter sounds reasonable i.e., instead of always allocating the first expansion, we'd always allocate the bindings snapshot.

---

_@dhruvmanila reviewed on 2025-08-21 06:08_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1331 on 2025-08-21 06:08_

Hmm, but then there's complication on restoring the snapshots at the correct places. I think I'll leave this as is for now.

---

_Merged by @dhruvmanila on 2025-08-21 06:13_

---

_Closed by @dhruvmanila on 2025-08-21 06:13_

---

_Branch deleted on 2025-08-21 06:13_

---
