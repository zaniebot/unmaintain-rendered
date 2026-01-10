```yaml
number: 902
title: "Incomplete equivalence of `*args, **kwargs` signature to `...` signature"
type: issue
state: closed
author: github-actions
labels:
  - bug
  - help wanted
  - type properties
assignees: []
created_at: 2025-07-27T12:06:50Z
updated_at: 2025-09-08T19:22:41Z
url: https://github.com/astral-sh/ty/issues/902
synced_at: 2026-01-10T02:06:24Z
```

# Incomplete equivalence of `*args, **kwargs` signature to `...` signature

---

_Issue opened by @github-actions on 2025-07-27 12:06_

All four reveal-types at the end of [this example](https://play.ty.dev/b545bd7a-0c30-45ae-8006-f94c6c23dd4d) should reveal `Literal[True]`:

```py
from typing import Callable, reveal_type
from ty_extensions import Intersection, Unknown, CallableTypeOf, Not, is_assignable_to

def f(*n1, **n2) -> None: ...

type A = Not[Callable[..., None]]
type B = Not[CallableTypeOf[f]]

def g(a: A, b: B):
    reveal_type(a)
    reveal_type(b)

reveal_type(is_assignable_to(A, A | B))  # revealed: Literal[True]
reveal_type(is_assignable_to(A, B | A))  # revealed: Literal[False]
reveal_type(is_assignable_to(B, A | B))  # revealed: Literal[False]
reveal_type(is_assignable_to(B, B | A))  # revealed: Literal[True]
```

Originally revealed by property tests in this run: https://github.com/astral-sh/ty/actions/runs/16550887703

---

_Label `bug` added by @github-actions[bot] on 2025-07-27 12:06_

---

_Label `type properties` added by @github-actions[bot] on 2025-07-27 12:06_

---

_Comment by @AlexWaygood on 2025-07-27 12:19_

```
 failures:

---- types::property_tests::stable::all_type_pairs_are_assignable_to_their_union stdout ----

thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [], neg: [Callable { params: GradualForm, returns: None }] }, Intersection { pos: [], neg: [Callable { params: List([Param { kind: Variadic, name: Some(Name("n1")), annotated_ty: None, default_ty: None }, Param { kind: KeywordVariadic, name: Some(Name("n2")), annotated_ty: None, default_ty: None }]), returns: None }] })
```

---

_Comment by @AlexWaygood on 2025-07-27 12:29_

I tried to reproduce this in the playground, but could not:

```py
from typing import Callable, reveal_type
from ty_extensions import Intersection, Unknown, CallableTypeOf, Not, is_assignable_to

def f(*n1, **n2): ...

type A = Not[Callable[..., Unknown]]
type B = Not[CallableTypeOf[f]]

reveal_type(is_assignable_to(A, A | B))
reveal_type(is_assignable_to(A, B | A))
reveal_type(is_assignable_to(B, A | B))
reveal_type(is_assignable_to(B, B | A))
```

All four `reveal_type` calls there reveal `Literal[True]`. https://play.ty.dev/424c52ef-3442-4c45-8ffd-6e0b703860f4

---

_Comment by @jelle-openai on 2025-07-28 15:00_

Is the issue that ty simplifies a type of the form `(*n1, **n2) -> Any` into `(...) -> Any`? The property test result seems to indicate it's using a form with explicit arguments. The two types should be equivalent but maybe you're not consistently simplifying from one to the other.

(Obligatory plug: pycroscope has a `dump_value()` [function](https://github.com/JelleZijlstra/pycroscope/blob/0d19236e4eda771175170a6b165b0e9f6a211d19/pycroscope/value.py#L363) that is like `reveal_type()` but prints out a more internal-style representation for the object. That can be useful in debugging exactly what the internal type representation is.)

---

_Comment by @AlexWaygood on 2025-07-28 15:06_

> (Obligatory plug: pycroscope has a `dump_value()` [function](https://github.com/JelleZijlstra/pycroscope/blob/0d19236e4eda771175170a6b165b0e9f6a211d19/pycroscope/value.py#L363) that is like `reveal_type()` but prints out a more internal-style representation for the object. That can be useful in debugging exactly what the internal type representation is.)

yes, I was wishing for this feature just yesterday while working on https://github.com/astral-sh/ty/issues/889 :-) It could be coming soon...

---

_Comment by @sharkdp on 2025-07-29 08:24_

I also longed for something like `ty_extensions.reveal_type_debug` (or similar) a lot of times. I think it should be possible to implement this "safely" now [with Doug's recent chances](https://github.com/astral-sh/ruff/pull/19400).

Somewhat related: I was thinking about separating the functions in `ty_extensions` into "potentially user facing" and "definitely internal". We might eventually expose something like `Intersection`, `Not` or `static_assert` to users. But `reveal_type_debug` should definitely be in the latter category, as the output should not be relied upon. Maybe it makes sense to have `ty_extensions` and `ty_extensions.internal` or similar.

---

_Comment by @carljm on 2025-08-05 02:39_

@AlexWaygood 's attempted repro differs from the property test failure in using `Unknown` instead of `None` as the return types. If I use `None`, the issue repros: https://play.ty.dev/a2a25c59-6813-4b54-9398-2e6130716b5c

```py
from typing import Callable, reveal_type
from ty_extensions import Intersection, Unknown, CallableTypeOf, Not, is_assignable_to

def f(*n1, **n2) -> None: ...

type A = Not[Callable[..., None]]
type B = Not[CallableTypeOf[f]]

def g(a: A, b: B):
    reveal_type(a)
    reveal_type(b)

reveal_type(is_assignable_to(A, A | B))  # revealed: Literal[True]
reveal_type(is_assignable_to(A, B | A))  # revealed: Literal[False]
reveal_type(is_assignable_to(B, A | B))  # revealed: Literal[False]
reveal_type(is_assignable_to(B, B | A))  # revealed: Literal[True]
```

---

_Label `help wanted` added by @carljm on 2025-08-05 02:39_

---

_Renamed from "Daily property test run failed on Sun Jul 27 2025" to "Incomplete equivalence of `*args, **kwargs` signature to `...` signature" by @carljm on 2025-08-05 02:40_

---

_Comment by @AlexWaygood on 2025-08-05 06:58_

Oh, I was probably getting confused between Rust `None` and Python `None` again 

---

_Comment by @GDYendell on 2025-09-07 17:49_

I have been investigating this and I am quite confused. This isn't testing equivalence of `*args, **kwargs` signature with `...` signature. It is just testing that the elements of a union are always assignable to the union. It doesn't matter what the types are. The use of `Not` in the types also seems irrelevant. This simplified version demonstrates the issue described by the title:

```py
from typing import Callable, reveal_type
from ty_extensions import CallableTypeOf, is_assignable_to

def f(*n1, **n2) -> None: ...

type A = Callable[..., None]
type B = CallableTypeOf[f]

reveal_type(is_assignable_to(A, B))  # revealed: Literal[True]
reveal_type(is_assignable_to(B, A))  # revealed: Literal[True]
```

However, I have just updated to main and now this also works ([playground](https://play.ty.dev/a2a25c59-6813-4b54-9398-2e6130716b5c)), as do the two original playground links.

I think the union issue was fixed between 5th August and 16th August (where my dev branch was based) and the function signature issue was fixed between 16th August and now.



---

_Comment by @carljm on 2025-09-08 19:22_

@GDYendell Thanks for the investigation!

We do generally support that any element of a union is assignable to the union; when we have failures in that invariant, it means that we've implemented _subtyping_ incorrectly (as opposed to assignability), because we use subtyping to simplify unions. So we might simplify the union `A | B` incorrectly to `A` (based on thinking that `B` is a subtype of `A`), and then find that `B` is not assignable to `A`. So I don't think there was any generalized "union issue" fixed here, but some issue specifically related to subtyping between `...` and `*a, **kw` callables.

In any case, happy to close this on the basis that we seem to have fixed all the examples shown here!

---

_Closed by @carljm on 2025-09-08 19:22_

---
