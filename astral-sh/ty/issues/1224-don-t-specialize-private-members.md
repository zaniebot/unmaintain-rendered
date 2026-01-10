```yaml
number: 1224
title: "don't specialize \"private\" members"
type: issue
state: open
author: KotlinIsland
labels:
  - unsoundness
assignees: []
created_at: 2025-09-21T03:14:20Z
updated_at: 2025-10-11T01:01:23Z
url: https://github.com/astral-sh/ty/issues/1224
synced_at: 2026-01-10T02:06:25Z
```

# don't specialize "private" members

---

_Issue opened by @KotlinIsland on 2025-09-21 03:14_

it's unsafe to specialize private members:
```py
class A[T]: # covariant
    def __init__(self, t: T):
         self._t = t

    def get(self) -> T:
        return self._t

    def f(self):
        a = A(1)
        b: A[object] = a
        b._t = "what?"
        a._t + 1 # runtime error
```

this can also manifest with annotated `self`:
```py
class A[T]:
    def __init__(self, t: T):
        self._t = t
    
    def get(self) -> T:
        return self._t

    def set(self: A[object], value: str):
        self._t = value  # expect error

    
a = A[int](1)
a.set("a string???, i hope it doesn't set `_t`")
a.get() + 1
```

this also applies to contravariant type parameters and private methods

if we instead never specialize private members, then we can only interact them safely with generic values, preventing this unsoundness

---

_Comment by @sharkdp on 2025-09-22 09:25_

Thank you for reporting this.

Can you please add some description here to explain why you expect there to be an error? I'm not arguing there shouldn't be none, but it would be easier to go through your reports if we wouldn't have to infer the intentions from the code.

(I have not started to understand what's going on here, but mypy, pyright, pyrefly also see no problem here)

---

_Comment by @KotlinIsland on 2025-09-22 13:37_

sure sorry

---

_Comment by @sharkdp on 2025-09-22 14:16_

It seems to me that the problem is the wrongly declared variance of `T`? If you declare `T` to be invariant or [write this example with PEP 695 generics](https://play.ty.dev/ec5b8c0f-fb21-4bdf-80c3-8a88dc726ae8), the error would be caught. So is the specific request here that we attempt to detect the inconsistency between the declared and the inferred variance of `T` in `A`?

---

_Comment by @KotlinIsland on 2025-09-22 16:04_

> wrongly declared variance of `T`

I don't understand, this class is covariant to `T`

no idea why using generic syntax incorrectly infers the variance

---

_Comment by @carljm on 2025-09-22 18:34_

There is a convention (implemented by other type checkers and ty) that underscore-prefixed ("private") mutable attributes can be considered compatible with covariance. (This implicitly depends on the idea that such attributes are never mutated externally, which not all type checkers enforce, and ty doesn't - yet - either.)

What this example demonstrates is that this convention is unsound when combined with a method that explicitly annotates `self` to a particular specialization of the containing generic class, and that method mutates the private attribute.

Ty currently considers that explicitly-specialized annotation of `self` to make the class invariant in `T`. This does prevent the unsoundness (and would for legacy typevars too, when we error on uses of a legacy typevar incompatible with their declared variance), but it may be overly aggressive for cases where such a method doesn't mutate any private attributes?

I think the choices are either that, or accept the unsoundness (as all other type checkers currently do), or implement some kind of more sophisticated approach. One possibility would be to error when methods annotated with an explicitly-specialized `self` type mutate private attributes (effectively consider the bodies of these methods to be "external" to the class.) This appears to be roughly what the OP is requesting (inferring from the location of the requested error.)

Would like to re-emphasize this point:

> it would be easier to go through your reports if we wouldn't have to infer the intentions from the code

If the point of a report is simply "this is unsound, I'm not sure what the right fix is", it's useful even to say that much.

---

_Comment by @carljm on 2025-09-22 18:36_

For some of these reports, where the unsoundness exists in all current type-checkers too, https://github.com/JelleZijlstra/unsoundness/ may be a better destination for the report. I'm not in principle opposed to having such issues open on ty, but they will likely be low priority for us as long as they are unsound in all other type checkers too.

---

_Label `unsoundness` added by @carljm on 2025-09-22 18:39_

---

_Comment by @KotlinIsland on 2025-09-23 00:02_

i would expect that there would be an error on the assignment of a value of `int` to `self._t`, when `self` is not `cls[T]`

i understand that there are limitations and unsoundness in the type system (like how `FunctionType` is not a valid `Callable`), but this case is very straightforward imo

---

_Comment by @carljm on 2025-09-23 00:05_

> i would expect that there would be an error on the assignment of a value of `int` to `self._t`, when `self` is not `cls[T]`

I don't think this is straightforward at all. What is the general principle by which a type checker should decide to emit this error?

In normal code, assigning an `int` to an attribute typed as `T`, when the instance is specialized with `T = object`, would be valid. Why should a type checker consider it invalid in this particular case?

---

_Comment by @KotlinIsland on 2025-09-23 01:54_

> In normal code, assigning an int to an attribute typed as `T`, when the instance is specialized with `T = object`, would be valid. Why should a type checker consider it invalid in this particular case?

because it's a so-called "private" variable, that has different interactions with variance

---

_Renamed from "variance not respected for annotated self method" to "don't materialize "private" members" by @KotlinIsland on 2025-10-01 01:30_

---

_Comment by @KotlinIsland on 2025-10-01 01:32_

@carljm i've updated the op with extra details to describe more cases and a potential solution

---

_Renamed from "don't materialize "private" members" to "don't specialize "private" members" by @KotlinIsland on 2025-10-01 04:04_

---

_Comment by @jorenham on 2025-10-10 20:13_

> > In normal code, assigning an int to an attribute typed as `T`, when the instance is specialized with `T = object`, would be valid. Why should a type checker consider it invalid in this particular case?
> 
> because it's a so-called "private" variable, that has different interactions with variance

There's even a conformance test for this:
https://github.com/python/typing/blob/ecc212940ea9dd794604a5b5a68af38a95400d8f/conformance/tests/generics_variance_inference.py#L70-L80

---

_Comment by @carljm on 2025-10-10 22:26_

@jorenham ty already implements the special covariance exception for private variables, and passes the conformance test you linked. What is requested in this issue is something beyond that, which is not specified and which no Python type checker currently implements, which is to add a new kind of check around modifications to private variables to help close some of the soundness loopholes resulting from that covariance exception.

---

_Comment by @jorenham on 2025-10-11 01:01_

> [@jorenham](https://github.com/jorenham) ty already implements the special covariance exception for private variables, and passes the conformance test you linked. What is requested in this issue is something beyond that, which is not specified and which no Python type checker currently implements, which is to add a new kind of check around modifications to private variables to help close some of the soundness loopholes resulting from that covariance exception.

Yea I was aware of that. I just wanted to elaborate on the reason for why it "private" attributes are special-cased in the context of variance inference, by showing that it's something defined in the typing spec, and therefore indeed not the point of this issue.

---
