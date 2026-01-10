```yaml
number: 1273
title: a way to explicitly specify variance using the new generic syntax
type: issue
state: open
author: DetachHead
labels:
  - wish
  - generics
assignees: []
created_at: 2025-09-29T05:00:49Z
updated_at: 2025-11-18T16:10:39Z
url: https://github.com/astral-sh/ty/issues/1273
synced_at: 2026-01-10T01:58:59Z
```

# a way to explicitly specify variance using the new generic syntax

---

_Issue opened by @DetachHead on 2025-09-29 05:00_

the new generic syntax is got a significant downgrade compared to the old syntax where you can no longer explicitly specify the variance.

it would be nice if ty had a way to work around this, eg:

```py
from ty_extensions import In, Out, InOut

class Foo[In[T]]: ... # contravariant
class Foo[Out[T]]: ... # covariant
class Foo[InOut[T]]: ... # invariant
```

(the names `In` and `Out` are taken from other languages like typescript, but perhaps for readability they could just be called `Covariant`, `Contravariant`, etc)

semi-related: #1017

---

_Label `wish` added by @sharkdp on 2025-09-29 06:54_

---

_Label `generics` added by @sharkdp on 2025-09-29 06:54_

---

_Comment by @AlexWaygood on 2025-10-01 11:44_

Thanks for the feature request. Unfortunately your proposed way of spelling this is invalid syntax, so -- while I think in general we're open to adding experimental special forms where they can help move the type system forward -- this specific spelling wouldn't work, as it would be (correctly) rejected by CPython's parser at runtime and also our parser:

```pycon
>>> class Foo[In[T]]: ...
  File "<python-input-0>", line 1
    class Foo[In[T]]: ...
                ^
SyntaxError: invalid syntax
```

I'm also not sure I agree that this kind of special form would move the type system forward, however, I'm afraid. It was a deliberate decision -- discussed at some length -- to avoid adding a way to explicitly specify variance as part of PEP-695. My impression is that a significant majority of Python developers are very happy that they no longer have to worry about the variance of type parameters when typing their code. There _are_ cases when you need to force a type checker to accept a typevar as being covariant even though it would "naturally" infer the typevar as invariant (or similar), but I think they're pretty rare and usually unsound. Personally, I'm happy to say that users should be forced to use the more verbose legacy syntax in the unusual situation that they need this kind of thing.

For now, I'm going to close this -- even if we did want to consider this feature request, we certainly wouldn't get to it any time soon, as there are many more pressing features that we still need to implement. But I'm happy to reconsider an alternative spelling if I can see evidence that it would be helpful to other developers too.

---

_Closed by @AlexWaygood on 2025-10-01 11:44_

---

_Comment by @DetachHead on 2025-10-01 12:42_

> Thanks for the feature request. Unfortunately your proposed way of spelling this is invalid syntax, so -- while I think in general we're open to adding experimental special forms where they can help move the type system forward -- this specific spelling wouldn't work, as it would be (correctly) rejected by CPython's parser at runtime and also our parser:

my bad, i should've checked that it was valid syntax before opening this lol

> I'm also not sure I agree that this kind of special form would move the type system forward, however, I'm afraid. It was a deliberate decision -- discussed at some length -- to avoid adding a way to explicitly specify variance as part of PEP-695. My impression is that a significant majority of Python developers are very happy that they no longer have to worry about the variance of type parameters when typing their code. There _are_ cases when you need to force a type checker to accept a typevar as being covariant even though it would "naturally" infer the typevar as invariant (or similar), but I think they're pretty rare and usually unsound. Personally, I'm happy to say that users should be forced to use the more verbose legacy syntax in the unusual situation that they need this kind of thing.

i'm not proposing that users are forced to specify the variance explicitly (though i would suggest an optional rule to do that), but i don't agree that it's a rare edge case to want to specify them. if you're developing a library i think it's essential to *always* specify the variance explicitly. otherwise you could unknowingly publish a breaking change to your API if you accidentally turn a class with a covariant generic into an invariant one by adding a new method.

i'm also convinced that while many users would consider it convenient that they don't have to think about variance, all hiding it does is make it more confusing when they encounter variance-related type errors.

---

_Comment by @AlexWaygood on 2025-10-01 12:51_

> i'm not proposing that users are forced to specify the variance explicitly

I know you weren't!

> but i don't agree that it's a rare edge case to want to specify them

but this is where I still disagree :-)

I sympathise with what you're saying about library development, but I think the vast majority of programmers using Python typing are not library developers! And I think even for library development, you can usually ensure that your type annotations are working as expected by adding tests that make use of functions such as `static_assert` and `assert_never`.

So I am still open to reconsidering this if we see meither more real-world use cases or more evidence that others agree with you. But for now I still see little value in reopening this :-)

---

_Comment by @DetachHead on 2025-10-09 10:25_

> otherwise you could unknowingly publish a breaking change to your API if you accidentally turn a class with a covariant generic into an invariant one by adding a new method.

to add to this i want to point out a recent example: the unintentional breaking change to frozen dataclasses introduced in python 3.13. see https://github.com/microsoft/pyright/discussions/11012

---

_Comment by @carljm on 2025-10-09 16:13_

@AlexWaygood I'd be inclined to leave this open in "wish" state; that makes it more findable for other developers who want to provide "real-world use cases" or "evidence that it would be helpful to other developers too".

I agree it likely won't be a priority for us anytime soon, absent a PEP to standardize it. (But I do find the library-developer use-case pretty convincing; it's a lot more verbose to write assertion tests about the variance of a library class than to specify the variance of a typevar.)

---

_Reopened by @carljm on 2025-10-09 16:13_

---

_Comment by @KotlinIsland on 2025-10-10 14:19_

```py
class A[T: Out, R: Out[int]]: ...
```

and they could be reused for use-site variance:

```py
def f(l: list[Out[object]]):
    l.append(1)  # expect error
```

---

_Comment by @jorenham on 2025-10-10 19:32_

As @carljm noted in https://discuss.python.org/t/should-variance-inference-ignore-underscore-prefixed-attributes/102978, it is unclear whether underscore-prefixed attributes should influence variance, as that would require the intent behind them. As far as I can tell, that's a pretty big class of use "real-world use cases" already.

---

_Comment by @carljm on 2025-10-10 22:33_

@jorenham Explicitly specifying variance of a typevar doesn't solve that problem, though -- you still need to agree on rules around the use of "private" variables, in exactly the same way, or else you just end up moving the errors to places where the type-checker says "you explicitly said this typevar is covariant, but you're using it contravariantly/invariantly." So I don't think this alone constitutes a use case for explicit specification of variance. (I do find "library authors want to ensure they don't accidentally break their own intentions" to be a reasonable use case.)

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
