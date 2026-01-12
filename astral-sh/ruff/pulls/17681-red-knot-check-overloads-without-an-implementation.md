```yaml
number: 17681
title: "[red-knot] Check overloads without an implementation"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: dhruv/overload-without-implementation
created_at: 2025-04-28T14:08:02Z
updated_at: 2025-04-30T14:24:23Z
url: https://github.com/astral-sh/ruff/pull/17681
synced_at: 2026-01-12T15:56:03Z
```

# [red-knot] Check overloads without an implementation

---

_@dhruvmanila_

## Summary

As mentioned in the spec (https://typing.python.org/en/latest/spec/overload.html#invalid-overload-definitions), part of #15383:

> The `@overload`-decorated definitions must be followed by an overload implementation, which does not include an `@overload` decorator. Type checkers should report an error or warning if an implementation is missing. Overload definitions within stub files, protocols, and on abstract methods within abstract base classes are exempt from this check.

## Test Plan

Remove TODOs from the test; create one diagnostic snapshot.


---

_Label `red-knot` added by @dhruvmanila on 2025-04-28 14:08_

---

_Label `diagnostics` added by @dhruvmanila on 2025-04-28 14:08_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-28 14:08_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-28 14:08_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-28 14:08_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-28 14:08_

---

_Comment by @github-actions[bot] on 2025-04-28 14:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @dhruvmanila on 2025-04-29 14:33_

I need to check why this branch is failing on "porcupine" project in mypy-primer.

Edit: https://github.com/astral-sh/ruff/pull/17609#issuecomment-2839445740 for why this failure is happening.

---

_Comment by @dhruvmanila on 2025-04-29 16:17_

Fixed the failure in [`382b15b` (#17609)](https://github.com/astral-sh/ruff/pull/17609/commits/382b15bb176055486b0780e0ce8f2f2a1d4d8e26)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:350 on 2025-04-29 21:24_

Sorry to nit diagnostic messages, but I find this one slightly grammatically confusing, it sounds like it's saying that the implementation has to be outside a stub file. Maybe "Overloaded non-stub function `func` must have an implementation"? This is shorter. It does depend on an understanding that a "stub function" is either a function in a stub file, or a Protocol, or an abstract method. But I think that's a pretty intuitive definition of "stub function". (And the current message also fails to mention the Protocol or abstract method possibilities.)

---

_@carljm approved on 2025-04-29 21:26_

Looks great!

---

_@dhruvmanila reviewed on 2025-04-29 21:41_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:350 on 2025-04-29 21:41_

No problem!

> Maybe "Overloaded non-stub function `func` must have an implementation"? This is shorter. It does depend on an understanding that a "stub function" is either a function in a stub file, or a Protocol, or an abstract method. But I think that's a pretty intuitive definition of "stub function".

Yeah, I like this. I think we can add, if we want to, some additional "info" for the diagnostics like the ones done in [`not-iterable` diagnostics](https://github.com/astral-sh/ruff/blob/f11d9cb509fa823b9c52ff799db7077199472677/crates/red_knot_python_semantic/resources/mdtest/snapshots/for.md_-_For_loops_-_With_non-callable_iterator.snap#L35-L46). So, we could provide info on what a "stub function" is.

> (And the current message also fails to mention the Protocol or abstract method possibilities.)

I thought about protocol and abstract method cases but those will never raise a diagnostic because they don't require an implementation. It's something that we could recommend when we see an overloaded function outside a stub file without an implementation but I'm not sure how useful would that be.


---

_@carljm reviewed on 2025-04-29 22:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:350 on 2025-04-29 22:19_

I think for now just updating to "Overloaded non-stub function `func` must have an implementation" is fine, we can see later if it seems this needs any more explanation.

---

_Merged by @dhruvmanila on 2025-04-30 14:24_

---

_Closed by @dhruvmanila on 2025-04-30 14:24_

---

_Branch deleted on 2025-04-30 14:24_

---
