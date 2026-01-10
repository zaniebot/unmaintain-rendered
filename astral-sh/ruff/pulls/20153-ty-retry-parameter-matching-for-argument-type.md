```yaml
number: 20153
title: "[ty] Retry parameter matching for argument type expansion"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/retry-parameter-matching
created_at: 2025-08-29T14:12:27Z
updated_at: 2025-09-12T08:40:43Z
url: https://github.com/astral-sh/ruff/pull/20153
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Retry parameter matching for argument type expansion

---

_Pull request opened by @dhruvmanila on 2025-08-29 14:12_

## Summary

This PR addresses an issue for a variadic argument when involved in argument type expansion of overload call evaluation.

The issue is that the expansion of the variadic argument could result in argument list of different arity. For example, in `*args: tuple[int] | tuple[int, str]`, the expansion would lead to the variadic argument being unpacked into 1 and 2 element respectively. This means that the parameter matching that was performed initially isn't sufficient and each expanded argument list would need to redo the parameter matching again.

This is currently done by redoing the parameter matching directly, maintaining the state of argument forms (and the conflicting forms), and updating the `Bindings` values if it changes.

Closes: astral-sh/ty#735

## Test Plan

Update existing mdtest.


---

_Label `ty` added by @dhruvmanila on 2025-08-29 14:12_

---

_Comment by @github-actions[bot] on 2025-09-01 09:47_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-01 09:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "[ty] [WIP] Retry parameter matching for argument type expansion" to "[ty] Retry parameter matching for argument type expansion" by @dhruvmanila on 2025-09-02 16:13_

---

_Marked ready for review by @dhruvmanila on 2025-09-02 16:13_

---

_Review requested from @carljm by @dhruvmanila on 2025-09-02 16:13_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-09-02 16:13_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-09-02 16:13_

---

_Review requested from @dcreager by @dhruvmanila on 2025-09-02 16:13_

---

_Comment by @dhruvmanila on 2025-09-02 16:13_

(This isn't completely ready but I'd appreciate some initial feedback on the approach.)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:218 on 2025-09-03 02:08_

:+1: Yep this looks like a good approach! It should pull out the more precise length of each union element during expansion

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1915 on 2025-09-03 02:08_

I like the cleanup from introducing this new type, too

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1412 on 2025-09-03 02:09_

:100: 

---

_@dcreager reviewed on 2025-09-03 02:09_

---

_@carljm reviewed on 2025-09-03 02:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1412 on 2025-09-03 02:28_

I don't know if we need to reflect this in our comments, but I think the spec is not wrong here -- the difference is that in the spec "step 1" means only "eliminate impossible overloads due to arity mismatch" whereas our step 1 is bigger than that, it also includes "match arguments to parameters", which in the spec is implicitly part of step 2, not part of step 1. This is why we need to "repeat step 1" and the spec does not. It may be clearer to say that we don't separate step 1 and 2 in the same way the spec does. 

---

_@dhruvmanila reviewed on 2025-09-03 03:27_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1412 on 2025-09-03 03:27_

That's a good point, I didn't realize this subtle difference, I'll update the comment to reflect this.

---

_Comment by @dhruvmanila on 2025-09-04 07:47_

> ```diff
> openlibrary (https://github.com/internetarchive/openlibrary)
> + openlibrary/coverstore/archive.py:422:13: error[no-matching-overload] No overload of function `__new__` matches arguments
> - Found 711 diagnostics
> + Found 712 diagnostics
> ```

I'm not exactly sure what's wrong here. I've created a playground snippet which breaks the union into it's elements which is what the type expansion would retry with (https://play.ty.dev/1990e010-195f-4800-9e31-b77894e37dd3). It's only the `c` variable which has a fixed length of 2 because the outermost type is a `tuple` containing two elements.

---

_Review requested from @dcreager by @dhruvmanila on 2025-09-04 15:00_

---

_Comment by @carljm on 2025-09-08 21:10_

@dcreager I'm assuming this is in your review queue unless you request review from me.

---

_Review request for @carljm removed by @carljm on 2025-09-08 21:10_

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-10 07:01_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:2313 on 2025-09-11 13:50_

:heart: Love that we can get rid of this workaround!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:1019 on 2025-09-11 13:53_

I don't think there's anything we're using this for anymore, now that we're no longer special-casing `__add__` for tuples (#19636). So I'd say we can go ahead and remove it; we can always resurrect it from the git history if we need to.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:112 on 2025-09-11 13:56_

You might consider storing the new `ArgumentForms` in `Bindings` instead of separating it out into two separate slices. That way it would be stored consistently throughout this file. That might require adding an accessor method for external callers to get access to the `argument_forms`. And you'd replace `ArgumentForms::into_boxed_slice` with `ArgumentForms::shrink_to_fit`.

---

_@dcreager approved on 2025-09-11 13:57_

This is great, thank you for tackling this!

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:112 on 2025-09-12 08:31_

That sounds reasonable, thanks!

---

_@dhruvmanila reviewed on 2025-09-12 08:31_

---

_Merged by @dhruvmanila on 2025-09-12 08:40_

---

_Closed by @dhruvmanila on 2025-09-12 08:40_

---

_Branch deleted on 2025-09-12 08:40_

---
