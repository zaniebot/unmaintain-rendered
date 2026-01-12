```yaml
number: 19496
title: "[ty] Return a tuple spec from the iterator protocol"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/iterate-tuple
created_at: 2025-07-22T20:19:29Z
updated_at: 2025-07-23T21:11:46Z
url: https://github.com/astral-sh/ruff/pull/19496
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Return a tuple spec from the iterator protocol

---

_@dcreager_

This PR updates our iterator protocol machinery to return a tuple spec describing the elements that are returned, instead of a type. That allows us to track heterogeneous iterators more precisely, and consolidates the logic in unpacking and splatting, which are the two places where we can take advantage of that more precise information. (Other iterator consumers, like `for` loops, have to collapse the iterated elements down to a single type regardless, and we provide a new helper method on `TupleSpec` to perform that summarization.)

---

_Label `ty` added by @dcreager on 2025-07-22 20:19_

---

_Comment by @github-actions[bot] on 2025-07-22 20:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-22 21:10_

I moved this logic over into `Type::try_iterate`, but it seems inconsistent with how we interpret iterating over `Never` in e.g. a `for` loop.  Before, this would reveal `Never`:

```py
def _(never: Never):
    for x in never:
        reveal_type(x)  # revealed: Never
```

i.e., the body is never executed, just like when we iterate over the empty tuple:

```py
for x in ():
    reveal_type(x)  # revealed: Never
```

This is consistent with simplifying `tuple[Never, ...]` to `tuple[()]`.

The interpretation described here is that iterating over `Never` might produce an arbitrary number of elements.  That requires changing the `for` loop test to reveal `Unknown`.

To me, the interpretation _without_ this special case seems more intuitive, especially when you consider it in terms of the `for` loop example. @AlexWaygood, you added this special case in #19469. Were there examples in the ecosystem check of indexing into this kind of tuple, which necessitated this special case?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/unpacker.rs`:133 on 2025-07-22 21:12_

This special case isn't needed any more, since we can now interpret `str`'s iteration behavior in the typeshed correctly.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4645 on 2025-07-22 21:13_

I think this clause is moot (and has been for awhile), since all tuple instances are now modeled as a `Type::Tuple`

---

_Marked ready for review by @dcreager on 2025-07-22 21:14_

---

_Review requested from @carljm by @dcreager on 2025-07-22 21:14_

---

_Review requested from @AlexWaygood by @dcreager on 2025-07-22 21:14_

---

_Review requested from @sharkdp by @dcreager on 2025-07-22 21:14_

---

_@dcreager reviewed on 2025-07-22 21:14_

---

_@dcreager reviewed on 2025-07-22 21:16_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4645 on 2025-07-22 21:16_

nm, I was wrong! This is still needed

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-22 21:22_

I think I added the special case in #18987, not #19469 — it was to tackle false positives I saw in the ecosystem report for sympy on early versions of that PR. If you look at the revisions to the primer report in https://github.com/astral-sh/ruff/pull/18987#issuecomment-3013282630, the false positives for sympy are present in the penultimate version of the comment

---

_@AlexWaygood reviewed on 2025-07-22 21:22_

---

_@carljm reviewed on 2025-07-22 21:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-22 21:43_

To me the thing that seems wrong in this comment is that we simplify `tuple[Never, ...]` to `tuple[()]`. Shouldn't it simplify to `Never` instead, and then we wouldn't get false positives indexing into it?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-22 22:04_

> Shouldn't it simplify to `Never` instead

No because that would imply the type is uninhabited. `tuple[Never, ...]` is inhabited by the empty tuple `()`, the same as `list[Never]` cannot simplify to `Never` because it is inhabited by the empty list `[]`.

The situation for `Never` type arguments to homogeneous tuples here is closer to the situation for other generic containers than it is to the situation for heterogeneous tuples 

---

_@AlexWaygood reviewed on 2025-07-22 22:04_

---

_@carljm reviewed on 2025-07-22 22:10_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-22 22:10_

Hmm, that makes sense, but I do wonder if there is any practical benefit to that simplification. It seems like there is a practical cost, because `tuple[Never, ...]` is a type that can arise in unreachable code, and the indexing errors aren't desirable there. It seems like there might be only upside to just leaving `tuple[Never, ...]` as-is, instead of simplifying it?

---

_@AlexWaygood reviewed on 2025-07-23 10:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-23 10:50_

@dcreager added that particular simplification so I'll defer to him, but I'm a bit reluctant to give up on it. It feels obviously correct from the perspective of set-theoretic types.

But we probably don't need the somewhat unprincipled special case I added here that Doug was asking me about -- a better fix would probably just be to check here whether the node is reachable before emitting the diagnostic:

https://github.com/astral-sh/ruff/blob/b605c3e2327fbc7c16f7e26cdc1f53c47ce60344/crates/ty_python_semantic/src/types/infer.rs#L8421-L8439

I think the machinery @sharkdp added should allow us to query that realtively easily?

---

_@carljm reviewed on 2025-07-23 15:35_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-23 15:35_

I think currently in order to query reachability for a node we have to know in advance that that node might need its reachability queried, and proactively store it in semantic indexing. Which has a memory cost if the category of nodes we are adding is large.

---

_@carljm approved on 2025-07-23 15:44_

Looks good!

---

_@dcreager reviewed on 2025-07-23 17:12_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-23 17:12_

Another possibility would be to special-case that iterating `Never` gives `Never`, not `tuple[Never, ...]` or `tuple[Unknown, ...]`. That is, it is "not iterable", as opposed to "iterable returning no elements" or "iterable returning unknown elements". That would let us keep the simplification of `tuple[Never, ...]` to `tuple[()]`, which I agree seems consistent with first principles.

---

_@carljm reviewed on 2025-07-23 20:30_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-23 20:30_

If iterating `Never` were to emit a "not iterable" diagnostic, then I think we'd have to special-case avoiding that diagnostic in unreachable code.

---

_@dcreager reviewed on 2025-07-23 20:40_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:986 on 2025-07-23 20:40_

> But we probably don't need the somewhat unprincipled special case I added here that Doug was asking me about -- a better fix would probably just be to check here whether the node is reachable before emitting the diagnostic:

I've left the `Never` special case for now, and added a TODO to consider doing this as a replacement. (This also leaves in place the simplification of `tuple[Never, ...]` to `tuple[()]`.)

---

_Merged by @dcreager on 2025-07-23 21:11_

---

_Closed by @dcreager on 2025-07-23 21:11_

---

_Branch deleted on 2025-07-23 21:11_

---
