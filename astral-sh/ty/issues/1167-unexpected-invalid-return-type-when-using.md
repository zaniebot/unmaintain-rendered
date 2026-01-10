```yaml
number: 1167
title: "Unexpected `invalid-return-type` when using narrowed dict as argument to dataclass"
type: issue
state: closed
author: jbree
labels:
  - bug
assignees: []
created_at: 2025-09-10T16:44:21Z
updated_at: 2026-01-08T18:36:06Z
url: https://github.com/astral-sh/ty/issues/1167
synced_at: 2026-01-10T01:56:40Z
```

# Unexpected `invalid-return-type` when using narrowed dict as argument to dataclass

---

_Issue opened by @jbree on 2025-09-10 16:44_

### Summary

I expect the return value to be properly typed with no errors, because following the `if` condition, `res` should be known to be a `dict`

https://play.ty.dev/caceacc9-cb4a-4193-b021-d2f9a8723ee3


### Version

playground 8770b9550

---

_Label `bug` added by @carljm on 2025-09-10 16:58_

---

_Added to milestone `GA` by @carljm on 2025-09-10 16:58_

---

_Comment by @carljm on 2025-09-10 16:58_

Thanks for the report! We actually know too much about the type of `res` here -- we know not only that it is a dict, but also that it is always truthy (that is, not empty). In some cases this is useful; in this case it causes problems with invariant generics.

We should either eliminate `AlwaysFalsy` and `AlwaysTruthy` from unions/intersections when solving generics, or possibly always remove them from unions/intersections, after allowing them to possibly simplify away other types.

---

_Comment by @mtshiba on 2025-09-21 16:12_

I'll try to address this. Let me explain my understanding and clarify the problem.

I don't think it's generally wrong to infer that `Data(foo=res)` is `Data[dict[Unknown, Unknown] & ~AlwaysFalsy]`. My understanding is that it's okay for an instance of `dict[Unknown, Unknown] & ~AlwaysFalsy` to be empty at some point. The type `dict[Unknown, Unknown] & ~AlwaysFalsy` means "the set of instances of `dict` *or its subclasses*, excluding those that *always* evaluate to `False`."
This is a type that can't be simplified any further, and is fundamentally different from `dict[Unknown, Unknown]` (Note that a subclass `SubDict[K, V]` of `dict[K, V]` created using class syntax satisfies `SubDict[K, V] <: dict[K, V]` regardless of the variance of `K, V`). The problem is that this guarantee (`& ~AlwaysFalsy`) is unnecessarily strong, and can't be discarded once typed.

One idea here is to use a type context to tell that the expected type here is `Data[dict[Unknown, Unknown]]`, disabling narrowing on `res`.

---

_Comment by @carljm on 2025-09-22 21:56_

Yes, I agree with your analysis. And I agree also that using the return type annotation as a type context for the `return` expression would help in this particular case. Though I think there are other cases where we don't have an immediate type context where we may get more intuitive results by discarding `& AlwaysFalsy` or `& AlwaysTruthy`.

---

_Comment by @carljm on 2026-01-08 18:36_

This is now fixed.

---

_Closed by @carljm on 2026-01-08 18:36_

---
