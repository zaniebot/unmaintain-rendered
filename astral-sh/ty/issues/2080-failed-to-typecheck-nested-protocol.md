```yaml
number: 2080
title: Failed to typecheck nested protocol
type: issue
state: open
author: xxchan
labels:
  - Protocols
assignees: []
created_at: 2025-12-18T16:52:11Z
updated_at: 2025-12-18T18:22:37Z
url: https://github.com/astral-sh/ty/issues/2080
synced_at: 2026-01-12T15:54:26Z
```

# Failed to typecheck nested protocol

---

_@xxchan_

### Summary

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Outer(Protocol):
    name: str
    
    class Inner(Protocol):
        def foo(self) -> int: ...
    
    def get_inner(self) -> Inner: ...

class MyOuter:
    name: str = "test"
    
    class Inner:
        def foo(self) -> int:
            return 1
    
    def get_inner(self) -> Inner:
        return MyOuter.Inner()

def use_outer(o: Outer) -> None:
    pass

use_outer(MyOuter())  # ty error, pyright passes
```

```
Exit code 1
error[invalid-argument-type]: Argument to function `use_outer` is incorrect
  --> /tmp/ty_nested_with_inner.py:25:11
   |
23 |     pass
24 |
25 | use_outer(MyOuter())
   |           ^^^^^^^^^ Expected `Outer`, found `MyOuter`
   |
info: Function defined here
  --> /tmp/ty_nested_with_inner.py:22:5
   |
20 |         return MyOuter.Inner()
21 |
22 | def use_outer(o: Outer) -> None:
   |     ^^^^^^^^^ -------- Parameter declared here
23 |     pass
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.3 (fadfe0966 2025-12-17)

---

_Label `Protocols` added by @carljm on 2025-12-18 17:06_

---

_Comment by @carljm on 2025-12-18 17:07_

Thanks for the report! Looks like a bug. Removing the `get_inner()` methods does not fix the error, so the issue is not with the use of the nested protocols, it's that we don't think `MyOuter.Inner` itself satisfies `Outer.Inner`. I suspect this is related to #903.

---

_Added to milestone `Stable` by @carljm on 2025-12-18 17:07_

---

_Comment by @AlexWaygood on 2025-12-18 17:59_

FWIW, while mypy and pyright are okay with this, pyrefly also rejects this code.

If we add a call to our helper function `reveal_protocol_interface` here, it's easy to see what's going on:

```py
from typing import Protocol, runtime_checkable
from ty_extensions import reveal_protocol_interface

@runtime_checkable
class Outer(Protocol):
    name: str
    
    class Inner(Protocol):
        def foo(self) -> int: ...

    def get_inner(self) -> Inner: ...

class MyOuter:
    name: str = "test"

    class Inner:
        def foo(self) -> int:
            return 1

    def get_inner(self) -> Inner:
        return MyOuter.Inner()

def use_outer(o: Outer) -> None:
    pass

# Revealed protocol interface: `{"Inner": AttributeMember(`<class 'Inner'>`), "get_inner": MethodMember(`(self, /) -> Inner`), "name": AttributeMember(`str`)}`
reveal_protocol_interface(Outer)
use_outer(MyOuter())  # ty error, pyright passes
```

In other words, ty has inferred that the protocol `Outer` has a member `Inner`, where the type of the `Inner` member is `<class 'Outer.Inner'>`. Instances of `MyOuter`, meanwhile, have an `Inner` attribute available, but that `Inner` attribute does not have a type that is assignable to `<class Outer.Inner>`. There is only one runtime value that inhabits the type `<class Outer.Inner>`, and that is the class `Outer.Inner`.

I think that this is a reasonable interpretation of the user's code here, given that it's totally unspecified what type checkers are meant to do if they see nested classes inside `Protocol` classes. We should probably consider emitting an error when we see nested classes inside `Protocol` classes, as part of our [`ambiguous-protocol-member`](https://docs.astral.sh/ty/reference/rules/#ambiguous-protocol-member) check.

I think the correct way to write this code would be to move the `Inner` protocol to the global namespace:

```py
from typing import Protocol, runtime_checkable

class Inner(Protocol):
    def foo(self) -> int: ...

@runtime_checkable
class Outer(Protocol):
    name: str
    
    def get_inner(self) -> Inner: ...

class MyOuter:
    name: str = "test"

    class Inner:
        def foo(self) -> int:
            return 1

    def get_inner(self) -> Inner:
        return MyOuter.Inner()

def use_outer(o: Outer) -> None:
    pass

use_outer(MyOuter())
```

This modified version of the snippet passes ty, pyright, mypy and pyrefly.

---

_Comment by @carljm on 2025-12-18 18:10_

I also think the mypy/pyright interpretation (which I roughly guess to be that a nested protocol class in a protocol is effectively the same as `Inner: ClassVar[type[GloballyDefinedInner]]` as an attribute on the protocol) is a reasonable interpretation of this, but I agree this is not specified anywhere.

Your version also makes an assumption that the `Inner` attribute itself is not intended to be part of the protocol, but it may be that the user intent of the OP is that implementers of the protocol are required to have an `Inner` class implementing the `Inner` protocol.

If we get reports indicating that this usage is common, then it seems worth matching the mypy/pyright behavior. If not, maybe not. For now, I'll take this out of stable milestone but leave it open?

---

_Removed from milestone `Stable` by @carljm on 2025-12-18 18:11_

---

_Comment by @AlexWaygood on 2025-12-18 18:22_

> Your version also makes an assumption that the `Inner` attribute itself is not intended to be part of the protocol, but it may be that the user intent of the OP is that implementers of the protocol are required to have an `Inner` class implementing the `Inner` protocol.

Right. A variant that does not make that assumption, but still passes all four type checkers, would be:

```py
from typing import Protocol, runtime_checkable

class InnerProtocol(Protocol):
    def foo(self) -> int: ...

@runtime_checkable
class Outer(Protocol):
    name: str

    @property
    def Inner(self) -> type[InnerProtocol]: ...
    
    def get_inner(self) -> InnerProtocol: ...

class MyOuter:
    name: str = "test"

    class Inner:
        def foo(self) -> int:
            return 1

    def get_inner(self) -> Inner:
        return MyOuter.Inner()

def use_outer(o: Outer) -> None:
    pass

use_outer(MyOuter())
```

I tried doing something like this, but if you do this then mypy/pyright/pyrefly all complain that `Outer` declares its `Inner` attribute must be settable, and apparently they all see `MyOuter.Inner` as implicitly read-only? Not sure why

```py
class InnerProtocol(Protocol):
    def foo(self) -> int: ...

@runtime_checkable
class Outer(Protocol):
    name: str

    Inner: type[InnerProtocol]
    
    def get_inner(self) -> InnerProtocol: ...
```

---
