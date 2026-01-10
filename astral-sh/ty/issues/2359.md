---
number: 2359
title: "No bidirectional inference for inferring the parameter types of `lambda` functions"
type: issue
state: closed
author: hyperkai
labels:
  - bug
assignees: []
created_at: 2026-01-06T09:32:25Z
updated_at: 2026-01-06T17:16:49Z
url: https://github.com/astral-sh/ty/issues/2359
synced_at: 2026-01-10T01:51:14Z
---

# No bidirectional inference for inferring the parameter types of `lambda` functions

---

_Issue opened by @hyperkai on 2026-01-06 09:32_

### Summary

*Memo:
- ty check
- ty 0.0.7
- Python 3.14.0

Using the type alias created by the [type statement](https://docs.python.org/3/reference/simple_stmts.html#the-type-statement) with `**P`, ty doesn't work properly, giving no error for `v2` as shown below:

*Memo:
- mypy, pyright and pyrefly work properly, giving error for `v2`.

```python
from collections.abc import Callable

type TA[**P] = Callable[P, None]

lam: TA[int] = lambda x: print(x)

v1: TA[int] = lam # No error
v2: TA[str] = lam # No error
```

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2026-01-06 09:34_

---

_Label `type aliases` added by @AlexWaygood on 2026-01-06 09:34_

---

_Renamed from "Using the type alias created by the type statement with `**P`, ty doesn't work properly" to "PEP-695 type aliases generic over ParamSpecs are not supported " by @AlexWaygood on 2026-01-06 09:35_

---

_Label `type aliases` removed by @AlexWaygood on 2026-01-06 12:46_

---

_Renamed from "PEP-695 type aliases generic over ParamSpecs are not supported " to "No bidirectional inference for inferring the parameter types of `lambda` functions" by @AlexWaygood on 2026-01-06 12:52_

---

_Comment by @AlexWaygood on 2026-01-06 12:56_

Thanks for the report! This actually doesn't seem to have anything to do with type aliases, but it has everything to do with lambdas.

We can see that if we use a function defined using a `def` statement, ty has the behaviour you'd expect here:

```py
from collections.abc import Callable

type TA[**P] = Callable[P, None]

lam: TA[int] = lambda x: print(x)
reveal_type(lam)  # (x) -> Unknown
v1: TA[int] = lam # No error
v2: TA[str] = lam # No error

def f(x: int, /) -> None: pass

not_lam: TA[int] = f
reveal_type(not_lam)  # def f(x: int, /) -> None
v3: TA[int] = not_lam
v4: TA[str] = not_lam  # error, as expected
```

What's happening is that for your `lambda` case, we don't yet use the annotated type of the variable (or any other type context) to inform how we should infer the type of a `lambda`'s parameter types. This means that the inferred type of the `lam` variable is `(x) -> Unknown` despite the explicit type annotation of `TA[int]`. We allow an object with type `(x) -> Unknown` to be assigned to the variable annotated with `TA[int]`, because `(x) -> Unknown` is assignable to the type `Callable[[int], None]`, but that doesn't alter our locally inferred type for the `lam` variable.

Since type aliases generic over paramspecs seem to be working fine if you use a function defined via a `def` statement, this seems to be just a duplicate of #181

---

_Closed by @AlexWaygood on 2026-01-06 12:56_

---

_Comment by @carljm on 2026-01-06 16:53_

I think for the specific OP example, #136 would be sufficient even without any bidirectional typing of lambdas. With that change we'd just respect the annotated `TA[int]` type, and it wouldn't matter what we infer for the lambda in this case (beyond that we infer something assignable to `TA[int]`, which we already do).

But I think it's more common for lambdas to be used "inline", without this kind of explicitly annotated assignment to a callable type, so #181 is also important -- just not necessary for this example.

---

_Comment by @AlexWaygood on 2026-01-06 17:16_

> I think for the specific OP example, [#136](https://github.com/astral-sh/ty/issues/136) would be sufficient even without any bidirectional typing of lambdas. With that change we'd just respect the annotated `TA[int]` type, and it wouldn't matter what we infer for the lambda in this case (beyond that we infer something assignable to `TA[int]`, which we already do).

Yes, either change would fix this, of course

---
