```yaml
number: 236
title: principles for inferring the type of an expression that is a type error
type: issue
state: closed
author: carljm
labels: []
assignees: []
created_at: 2024-10-25T21:19:49Z
updated_at: 2025-12-19T23:54:08Z
url: https://github.com/astral-sh/ty/issues/236
synced_at: 2026-01-10T01:56:40Z
```

# principles for inferring the type of an expression that is a type error

---

_Issue opened by @carljm on 2024-10-25 21:19_

When we believe that an expression is a type error, and will raise at runtime, there are several plausible types we could assign to that expression:

a) We could assign the type `Unknown` (or `Any`).
b) We could assign the type `Never`.
c) We could assign the type that we believe the expression should always have, were it not an error.

It seems that existing type checkers use a mix of all three strategies, in different situations.

(a) is the most cautious approach. It suggests that we might be wrong in our analysis: perhaps the code actually works at runtime, but we just don't know what the type should be. All existing type checkers infer Unknown/Any for unrecognized imports. Type checkers (by necessity) have a very limited understanding of the Python import system, and may not be configured to be able to find all imports (especially imports of e.g. C extension modules, if type stubs are not available), so it makes sense to assume in this case that the import may be valid at runtime.

(b) is theoretically correct for an expression that we are confident will raise an exception at runtime. If the expression always raises, evaluation of the expression will always terminate, and thus the expression's type can have no inhabitant. Since `Never` is assignable to every type, and should be considered to have all attributes (also of type `Never`), this is also a reasonably "forgiving" approach (in the sense that it shouldn't result in lots of cascading errors.)

(c) is also a defensible approach in cases where the expression is of a form that should always have the same type, were it not an error. An example here is membership tests: `x in y` at runtime will _always_ be of type `bool`, if it doesn't raise an exception. So it's reasonable to simply assign it type `bool` even if we think the comparison will raise, on the assumption that the invalid test will be fixed, and thus the type `bool` gives the most accurate downstream type-checking results without requiring multiple iterations (first fix the error, then wait for re-check). Mypy infers `bool` for an invalid membership test, which is an example of this strategy.

I think it may make sense to use all three strategies sometimes, but we should have a clear understanding of our principles for deciding which to use in a given situation. I would propose something like the following:

1. If the expression form would _always_ result in a certain type if not an error, we just infer that type, on the assumption that the error will most likely be fixed.
2. Otherwise, we prefer inferring `Never` if we consider the error to be high-confidence, or `Unknown` if low-confidence (an unresolved import, for example.)

I'm not sure about part (2), though -- it feels difficult to consistently decide what errors are "high-confidence." Maybe there is little value in using `Never` in this way, and we should just generally prefer `Unknown`.

---

_Comment by @carljm on 2024-10-25 21:30_

There's also the case of "type errors" which we do _not_ believe will error at runtime, but violate the user's declared type intentions. For example, `x: int = "foo"` is not a runtime error, but is a type error, and we have to decide how to reconcile it. Our current choice is to preserve the inferred type (which is the type we will reveal, so `Literal["foo"]` for `x` in this case), and relax the declared type to `Unknown`, meaning we effectively ignore the `int` annotation and will subsequently allow anything to be assigned to `x`.

This choice makes sense in case the inferred type is very high confidence (as it is in this example), but could be wrong in a case like `x: int = annotated_to_return_str_but_really_returns_int_at_runtime()`. At the moment, I'm comfortable with this tradeoff: the second case will presumably emit two diagnostics (maybe only one if the bad annotation is in a type stub, or badly-annotated third-party library that we aren't checking), and once they are fixed the inferred and declared types for `x` will be correct. But we'll see how it holds up in real-world code.

Another option here could be to widen the declared type just enough to accommodate the observed assignment, so for `x: int = "foo"` we'd emit a diagnostic for invalid assignment and then widen the declared type of `x` to `int | Literal["foo"]`, which is the minimal adjustment needed to maintain the invariant that inferred type is always assignable to declared type. The pro of this is that if the `int` annotation was actually correct, this will mostly still respect it for subsequent assignments. The downside is if the annotation is wrong, you'll just get more bogus cascading errors. And it's potentially confusing: may not be clear where the type `int | Literal["foo"]` came from.

---

_Comment by @carljm on 2024-10-25 22:15_

A similar case is "invalid override" errors. We can emit a diagnostic if a class overrides a base class method unsafely (violates Liskov), but this is not a runtime error, so the best we can do is go ahead and use the unsafe method definition as written. (This is what both mypy and pyright do.)

---

_Comment by @AlexWaygood on 2024-10-26 18:45_

> This choice makes sense in case the inferred type is very high confidence (as it is in this example), but could be wrong in a case like `x: int = annotated_to_return_str_but_really_returns_int_at_runtime()`. At the moment, I'm comfortable with this tradeoff: the second case will presumably emit two diagnostics (maybe only one if the bad annotation is in a type stub, or badly-annotated third-party library that we aren't checking), and once they are fixed the inferred and declared types for `x` will be correct. But we'll see how it holds up in real-world code.

One option the user has here is to use `cast` to get around the incorrectly annotated function:

```py
from typing import cast

x: int = cast(int, annotated_to_return_str_but_really_returns_int_at_runtime())
```

But the disadvantage of this is that `cast()` is highly unsafe, and using it is often discouraged.

---

_Comment by @carljm on 2024-10-26 18:49_

Yes, good point. Also, using `cast` to work around a wrong annotation is no _more_ unsafe than a type checker choosing to always trust the annotation (and treat it like a cast) in case of an invalid assignment. And at least it makes the unsafety more clearly opt-in and explicit. 

---

_Renamed from "[red-knot] principles for inferring the type of an expression that is a type error" to "principles for inferring the type of an expression that is a type error" by @MichaReiser on 2025-05-07 15:27_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 15:48_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Comment by @carljm on 2025-12-19 23:54_

I think we already follow the principles discussed here pretty well (though we don't really ever use the "infer Never" option, which I think is OK). Going to close this since I don't think it's actionable.

---

_Closed by @carljm on 2025-12-19 23:54_

---
