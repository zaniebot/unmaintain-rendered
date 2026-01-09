---
number: 15672
title: "[red-knot] Stack overflow in `Type::bool`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2025-01-22T14:01:36Z
updated_at: 2025-02-04T20:40:08Z
url: https://github.com/astral-sh/ruff/issues/15672
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Stack overflow in `Type::bool`

---

_Issue opened by @sharkdp on 2025-01-22 14:01_

### Description

We have an existing code path in `Type::bool` that tries to prevent infinite recursion:

https://github.com/astral-sh/ruff/blob/13e7afca42e1ab4e698412df903e3dd172909caa/crates/red_knot_python_semantic/src/types.rs#L1777-L1786

and a test for it here:

https://github.com/astral-sh/ruff/blob/13e7afca42e1ab4e698412df903e3dd172909caa/crates/red_knot_python_semantic/resources/mdtest/unary/not.md?plain=1#L140-L145

But this only checks the case where `__bool__` has the exact type `Literal[bool]`. It fails if the type is a union that contains `Literal[bool]`, for example:

```py
def flag() -> bool: return True

class Boom:
    if flag():
        __bool__ = bool
    else:
        __bool__ = int

bool(Boom())
```

```
> cargo run --bin red_knot -- --project /path/to/folder/with/example
thread '<unknown>' has overflowed its stack
fatal runtime error: stack overflow
```

---

_Label `bug` added by @sharkdp on 2025-01-22 14:01_

---

_Label `red-knot` added by @sharkdp on 2025-01-22 14:01_

---

_Assigned to @sharkdp by @sharkdp on 2025-01-22 14:03_

---

_Referenced in [astral-sh/ruff#15674](../../astral-sh/ruff/pulls/15674.md) on 2025-01-22 15:48_

---

_Unassigned @sharkdp by @sharkdp on 2025-01-23 14:12_

---

_Label `help wanted` added by @sharkdp on 2025-01-23 14:12_

---

_Comment by @mishamsk on 2025-01-30 00:35_

@sharkdp hey. I wanted to work on red knot for some time. This one seems easy (at least on the surface). I'd try if you do not mind 

---

_Comment by @sharkdp on 2025-01-30 07:55_

> I'd try if you do not mind

Not at all, let us know if you need help with anything.

---

_Comment by @mmcqd on 2025-01-30 09:59_

I got curious and spent some time looking into this. The current behavior is actually wrong in more than just the way reported. 

In this code:
```
 class BoolIsBool: 
     __bool__ = bool 
```
`BoolIsBool` can actually be statically guaranteed to be `AlwaysFalsy`, which is the opposite of what calling `bool` would normally do! Try it out:
```
bool(BoolIsBool()) # evaluates to False
```

From looking [at the python source code](https://github.com/python/cpython/blob/main/Objects/typeobject.c#L9832), given `foo = Foo()`, when trying to to execute `bool(foo)`, python will look up `Foo`'s `__bool__` method and check whether it has the `Py_TPFLAGS_METHOD_DESCRIPTOR` flag. If the method object has this flag, then we invoke `Foo.__bool__(foo)`, but if it doesn't have this flag, we invoke `Foo.__bool__()`, with no arguments.

I don't perfectly understand what this flag does, but from looking over https://peps.python.org/pep-0590/#descriptor-behavior, https://github.com/python/cpython/pull/13338 and https://github.com/python/cpython/pull/13865, it's a generalization of checking "is this literally a function", but it's _not_ checking "is this callable", it's more specific than that. Crucially the class `bool` [_doesn't_ have this flag](https://github.com/python/cpython/blob/71aecc284efdf997939568a4167dbffe1a65b9bf/Objects/boolobject.c#L191).

So in the `BoolIsBool` example, `bool(BoolIsBool())` evaluates to `bool()`, which is `False`.

You can do a similar thing with any unary dunder method
```
class Foo:
    __neg__ = int

-Foo() # evaluates to int(), which evaluates to 0
```

I'm inclined to say that there should be an error (or a least warning) diagnostic produced during class checking if you try to assign a non-function object to one of these methods. Otherwise some special casing probably needs to be done in  `Type::call` for calling unary dunder methods whose values are class literals.

---

_Comment by @mishamsk on 2025-01-30 15:53_

@mmcqd thanks for this. Very good point. Quite an entertaining research too.

But in general, it kinda makes sense and falls inline with the descriptor protocol. Functions bound within the class normally implicitly become descriptors (CPython "creates" the `__get__` method which returns the bound method object). Or a descriptor is manually assigned (e.g. `@property` effectively creates and binds a descriptor object in class namespace).

That flag they introduced just avoids creating a wrapper around something that is already a descriptor.

But CPython also allows non-descriptor objects, which naturally means - not passing the `self` to the call. `bool` is not a descriptor and not a method definition - so we have that behavior.

I'll check if red_knot has some notion of descriptors already.

---

_Comment by @mishamsk on 2025-01-31 00:43_

correction. Apparently [all functions](https://docs.python.org/3.13/howto/descriptor.html#functions-and-methods), not just those defined in class bodies have `__get__` (wonder where I got the idea that only class functions do...).

Quick primer:

```
In [1]: def foo():
   ...:     pass

In [2]: foo.__get__
Out[2]: <method-wrapper '__get__' of function object at 0x10706be20>

In [3]: def foo(arg):
   ...:     print(arg)

In [4]: bfoo = foo.__get__("wo")

In [5]: bfoo()
wo
```

I also found a todo comment in red_knot method lookup for `__get__` left by @AlexWaygood - so I see some descriptor handling was planned.

For now my plan is to change `Type::call` signature or create a separate method to distinguish regular calls & when descriptor protocol will be invoked. Essentially following what @mmcqd linked in CPython source code. I'll open a PR soon

---

_Referenced in [astral-sh/ruff#15843](../../astral-sh/ruff/pulls/15843.md) on 2025-01-31 04:05_

---

_Closed by @carljm on 2025-02-04 20:40_

---
