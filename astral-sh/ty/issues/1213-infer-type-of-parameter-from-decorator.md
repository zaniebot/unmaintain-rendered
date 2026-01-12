```yaml
number: 1213
title: infer type of parameter from decorator
type: issue
state: open
author: KotlinIsland
labels:
  - wish
assignees: []
created_at: 2025-09-20T06:43:01Z
updated_at: 2025-09-23T02:03:18Z
url: https://github.com/astral-sh/ty/issues/1213
synced_at: 2026-01-12T15:54:24Z
```

# infer type of parameter from decorator

---

_@KotlinIsland_

### Summary

```py
from typing import Callable

def deco(fn: Callable[[int], None]) -> None: ...

@deco
def f(i):
    reveal_type(i)  # Unknown
```
here i would expect `int`

### Version

_No response_

---

_Comment by @sharkdp on 2025-09-22 07:24_

Thank you for reporting this.

This looks like something that is rather hard to implement, but has limited practical benefit? It also seems like no other type checker supports this?

I'm not even sure if it's necessarily correct? Even if we see the application of the decorator to the function as a constraint on its parameters, it should only impose a `type(i) :> int` subtyping constraint? So the type of `i` could be `int`, but it could also be a supertype of `int`.

---

_Label `wish` added by @sharkdp on 2025-09-22 07:24_

---

_Comment by @KotlinIsland on 2025-09-22 08:18_

> I'm not even sure if it's necessarily correct

it is, this construct is identical to:

```py
deco(lambda i: reveal_type(i))
```

and in this case it is quite clear to see that `i` should be `int`

> This looks like something that is rather hard to implement

i can't imagine it would be any harder to implement that the above case with a `lambda`

> It also seems like no other type checker supports this?

yes, noone else supports it, but i think this is a really slick feature

---

_Comment by @sharkdp on 2025-09-22 09:44_

> it is, this construct is identical to:
> 
> deco(lambda i: reveal_type(i))

I don't see how this transformation addresses my comment regarding subtyping?

---

_Comment by @KotlinIsland on 2025-09-22 10:02_

yes, your analysis is correct. it could be `int` or a super type of `int`

these same rules would apply to any `Callable`:
```py
class C:
    def __call__(self, i: object) -> None: ...
deco(C())
```

what i have meant is: this should be a single point of constraint solving that mirrors the same semantics observed with a lambda, or any callable

---

_Comment by @sharkdp on 2025-09-22 11:11_

> yes, your analysis is correct. it could be `int` or a super type of `int`

In this case, the original request does not seem viable to me. I guess we could infer `object`, but that wouldn't be any more helpful than `Unknown`.

I will close this for now. Let me know if I'm missing something (I didn't get why the discussion around function vs lambda vs custom callable is even relevant here).

---

_Closed by @sharkdp on 2025-09-22 11:11_

---

_Comment by @KotlinIsland on 2025-09-22 13:31_

i really don't understand your position

if the decorator is typed as accepting `(int) -> None`, then the value of `i` will __always__ be `int` or lower. i don't understand your comment about inferring `object`

would you infer `object` here?
```py
def f(fn: Callable[[int], None]): ...

f(lambda i: print(i))
```

does that make sense?

---

_Comment by @sharkdp on 2025-09-22 14:30_

> if the decorator is typed as accepting `(int) -> None`, then the value of `i` will **always** be `int` or lower

No, that was my point. The type of `i` could be `int` or *larger* (a supertype of `int`). There is nothing wrong with your original example if the function would assume `i` to be of type `object`. You could still apply an `(int) -> None` decorator to it. This is fine:

```py
from typing import Callable

def deco(fn: Callable[[int], None]): ...

@deco
def f1(i: int) -> None:
    reveal_type(i)  # int

@deco
def f2(i: object) -> None:
    reveal_type(i)  # object
```



---

_Comment by @KotlinIsland on 2025-09-22 14:52_

you could choose to annotate it with `object`, but what bearing does that have on the function?

could you explain why a user would ever want to annotate `i` as `object`

it's comparable to inferring `[1]` as `list[object]` because it's the widest type

actually, it's not like that, that would be ergonomic to certain use cases. but i can't see any case where it would beneficially affect the types to use `object`

---

_Comment by @KotlinIsland on 2025-09-22 16:18_

what you are suggesting is akin to:
```py
from typing import Callable

class A[T]:
    def __init__(self, t: T):


A(1) # infer A[Unknown] or A[object] because the type parameter wasn't specified

def deco(fn: Callable[[int], None]): ...

class C[T]:
    def __call__(self, t: T) -> None: ...


def deco(fn: Callable[[int], None]): ...

deco(C()) # C inferred as C[Unknown] because the type var wasn't specified 
```

---

_Comment by @carljm on 2025-09-22 20:04_

It's not about whether a user would want to explicitly annotate with `object`, it's just unsound (aka wrong) to assume that `i` (if un-annotated) must be `int` inside the body of `f`. The same is true whether `f` is a function or a lambda. Because `(object) -> None` is a subtype of `(int) -> None`, so the fact that `f` must be a subtype of `(int) -> None` does not tell us that `i` must be `int`; `f` could just as well have the type `(object) -> None`.

Here is a demonstration of the unsoundness. `i` will be `str` here at runtime. Where would you expect a type error in this example?

```py
from typing import Callable

def deco(fn: Callable[[int], None]): ...

@deco
def f(i):
    reveal_type(i)  # Unknown

f("foo")
```

---

_Comment by @KotlinIsland on 2025-09-22 23:04_

@carljm i think there has been a grave misunderstanding

in my case the decorator doesn't return the function

```py
from typing import Callable

def deco(fn: Callable[[int], None]) -> None:
    fn(1)

@deco
def f(i):
    reveal_type(i)  # int

deco(lambda i: reveal_type(i))
```

i guess this was an oversight on my behalf, i am too acustomed to unannotated equals `-> None`, this is why i kept saying it is identical to the lambda form

i hope this clears up any confusion, i've updated the op

---

_Comment by @carljm on 2025-09-22 23:54_

Hmm, I think it is true that if we assume the function/lambda can only ever be called via the callable type it is assigned to, then the parameter types of that callable type are in fact upper bounds on the types it can receive as arguments.

I think this assumption can be made for a lambda passed as callback to an annotated callback type.

It seems a bit trickier to establish the conditions under which the assumption is safe in the OP example with a decorator.

---

_Comment by @KotlinIsland on 2025-09-23 01:57_

> It seems a bit trickier to establish the conditions under which the assumption is safe in the OP example with a decorator.

could you explain why that is? as a decorator is just "syntatic sugar" for calling the decorator and passing a function, then assigning the result to a variable with the name of the function. so to my analysis they are purely equal

---

_Comment by @carljm on 2025-09-23 02:03_

Actually I think you're right that they are equivalent if decorator syntax is used -- once the function is passed into the decorator, although it could have "originally been" of type `(object) -> None` in principle, from that point on in practice it can never be called except "through" the type `(int) -> None`, which means it can only ever receive an `int` argument.

I'll reopen this, though I think it will be higher priority to do it for lambdas than for decorated functions, since you can't annotate the lambda.

---

_Reopened by @carljm on 2025-09-23 02:03_

---
