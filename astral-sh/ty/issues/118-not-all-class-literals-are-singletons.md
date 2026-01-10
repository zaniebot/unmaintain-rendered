```yaml
number: 118
title: Not all class literals are singletons
type: issue
state: closed
author: dcreager
labels:
  - type properties
assignees: []
created_at: 2025-04-21T20:59:32Z
updated_at: 2025-07-23T23:12:49Z
url: https://github.com/astral-sh/ty/issues/118
synced_at: 2026-01-10T02:06:24Z
```

# Not all class literals are singletons

---

_Issue opened by @dcreager on 2025-04-21 20:59_

I'm not sure if what this might break (if anything), but we currently assume that all class literals are singletons.  Normally they are:

```py
class C: ...
```

Only one instance of the type `C` at runtime. But if a class is defined in a function:

```py
def f():
    class D: ...
```

then a new `type` is created each time the function is invoked.

A couple of wrinkles complicate this:

- Inside the function body, `D` _is_ a singleton, since for the duration of that single invocation, `D` refers to the (single) class that was just created.

- It's only _outside_ the function body where we have to consider that `f` might be invoked multiple times. Now, at the moment, without return type inference (#17349), there isn't a way for anything outside of `f` to talk about `D` (either the type itself, or subclasses of it, or instances of it, etc). But we'll have to consider how we want to represent this if we do implement RTI.

(There's some overlap here with eager nested scopes, since that's another feature that changes behavior once we cross a function body boundary.)

---

_Comment by @carljm on 2025-04-21 22:22_

Seems like we may want to label scopes as run-once vs run-many (where the former is any scope outside of any function, and the latter is all scopes where any ancestor is a function), and then `Type::ClassLiteral` would know if its a singleton or not based on whether the scope in which it was defined is run-once or run-many?

---

_Renamed from "[red-knot] Not all class literals are singletons" to "Not all class literals are singletons" by @MichaReiser on 2025-05-07 15:25_

---

_Comment by @carljm on 2025-05-09 00:59_

I put a bit more thought into this over in https://github.com/astral-sh/ty/issues/128#issuecomment-2864800219 

I don't think my comment above makes sense; it's not nearly enough to just consider the class-literal type not a singleton; this affects all kinds of assumptions we make about class and instance types. I think instead we have to avoid this possibility in our implementation of return-type inference.

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:39_

---

_Comment by @carljm on 2025-07-23 23:12_

I think our conclusion here is to continue to assume that class literal types are singletons, and avoid allowing them to be inferred as a return type.

---

_Closed by @carljm on 2025-07-23 23:12_

---
