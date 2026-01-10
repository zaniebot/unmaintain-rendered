```yaml
number: 17607
title: "[red-knot] Use iterative approach to collect overloads"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/overload-iterative
created_at: 2025-04-24T13:34:42Z
updated_at: 2025-04-28T23:56:52Z
url: https://github.com/astral-sh/ruff/pull/17607
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Use iterative approach to collect overloads

---

_Pull request opened by @dhruvmanila on 2025-04-24 13:34_

## Summary

This PR updates the `to_overloaded` method to use an iterative approach instead of a recursive one.

Refer to https://github.com/astral-sh/ruff/pull/17585#discussion_r2056804587 for context.

The main benefit here is that it avoids calling the `to_overloaded` function in a recursive manner which is a salsa query. So, this is a bit hand wavy but we should also see less memory used because the cache will only contain a single entry which should be the entire overload chain. Previously, the recursive approach would mean that each of the function involved in an overload chain would have a cache entry. This reduce in memory shouldn't be too much and I haven't looked at the actual data for it.

## Test Plan

Existing test cases should pass.


---

_Label `internal` added by @dhruvmanila on 2025-04-24 13:34_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-24 13:34_

---

_Comment by @github-actions[bot] on 2025-04-24 13:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-04-24 15:11_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-24 15:11_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-24 15:11_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-24 15:11_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-24 15:11_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-24 15:12_

---

_Comment by @dcreager on 2025-04-24 15:16_

As an aside, can/should `FunctionSignature` use `CallableSignature` for its list of overloads, instead of `Vec<Signature>`?

---

_Comment by @dcreager on 2025-04-24 15:18_

And if we add `CallableSignature::is_overloaded`, you could use `CallableSignature` for the non-overloaded case, too, and then collapse the two variants of `FunctionSignature` down into a struct.

---

_@MichaReiser reviewed on 2025-04-24 15:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:6198 on 2025-04-24 15:49_

You could call `reverse` on vec instead of `into_iter().rev().collect()`

---

_@MichaReiser approved on 2025-04-24 15:50_

---

_Comment by @carljm on 2025-04-24 16:04_

> can/should `FunctionSignature` use `CallableSignature` for its list of overloads

I thought about this when reviewing a previous PR that introduced `FunctionSignature`, but decided not to comment on it at the time because `CallableSignature` has a number of extra fields that are not useful to `FunctionSignature`, and there didn't seem to be a strong downside to having `FunctionSignature` not use `CallableSignature`. I am interested in @dhruvmanila 's thoughts here, though.

---

_@carljm approved on 2025-04-24 16:06_

---

_Comment by @dhruvmanila on 2025-04-24 16:14_

> As an aside, can/should `FunctionSignature` use `CallableSignature` for its list of overloads, instead of `Vec<Signature>`?

Yeah, I think that could be used. Initially, I avoided that because of the additional fields that it had. There are couple of methods, specifically the `into_callable_type` on `FunctionType` and `BoundMethodType` which just requires the `Signature`s to convert the function / method into a callable type. So, the intermediary `CallableSignature` is not useful  in that case.

---

_Comment by @dhruvmanila on 2025-04-24 16:17_

> And if we add `CallableSignature::is_overloaded`, you could use `CallableSignature` for the non-overloaded case, too, and then collapse the two variants of `FunctionSignature` down into a struct.

Yeah, that's a good idea. I can try in a follow-up later to remove the `FunctionSignature` if possible.

---

_Closed by @dhruvmanila on 2025-04-24 16:47_

---

_Reopened by @dhruvmanila on 2025-04-24 16:47_

---

_Merged by @dhruvmanila on 2025-04-24 16:53_

---

_Closed by @dhruvmanila on 2025-04-24 16:53_

---

_Branch deleted on 2025-04-24 16:53_

---

_Comment by @dhruvmanila on 2025-04-28 23:56_

> Yeah, that's a good idea. I can try in a follow-up later to remove the `FunctionSignature` if possible.

Actually, I realized today why I didn't opt for that - it's because `CallableSignature` requires the `signature_type` as an input when creating the struct and in `FunctionType::signature` that would always be `FunctionType` which isn't correct. For a bound method it should be `Type::BoundMethod`, for a callable type it should be `Type::Callable`, and so on.

---
