```yaml
number: 166
title: Tracking issue for override-related checks (including the Liskov Substitution Principle)
type: issue
state: open
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-03-20T23:03:06Z
updated_at: 2025-12-22T13:02:31Z
url: https://github.com/astral-sh/ty/issues/166
synced_at: 2026-01-12T15:54:22Z
```

# Tracking issue for override-related checks (including the Liskov Substitution Principle)

---

_@carljm_

This means checking that subclass methods are subtypes of the superclass method they override, if any.

Subclass property getters must return a subtype of the corresponding superclass property getters, and subclass property setters must accept a supertype of the corresponding superclass property setter.

If we model any mutable attribute as conceptually a property with getter and setter, where the setter accepts the same type that the getter returns, this implies that the type of mutable attributes must be invariant; that is, subclasses may neither widen nor narrow the type of the attribute. Pyright [models this correctly](https://pyright-play.net/?code=MYGwhgzhAEBCkFMBc0B06BQpIwMoFcAjACnggQEoV1UMtwpoBhJDad6ADxTITu0YARYkypsO3aAUIYgA); mypy [does not](https://mypy-play.net/?mypy=latest&python=3.12&gist=7aa8cc84b99db33bc8bf58e083aee2bf).

See https://github.com/astral-sh/ty/issues/167 for a demonstration of a particular case involving ClassVar.

---

_Comment by @jorenham on 2025-03-21 12:14_

Note that the LSP does not apply to `Protocol` that's marked as `@final`, because it's a purely structural, and cannot be used nominally. Both pyright and mypy model this incorrectly at the moment.

---

_Comment by @carljm on 2025-03-21 16:12_

> Note that the LSP does not apply to `Protocol` that's marked as `@final`

If I understand correctly, you are describing a case like this?

```py
from typing import Protocol, final

class A(Protocol):
    x: int

@final
class B(A, Protocol):
    x: str
```

In this case it does seem intuitive to me to enforce Liskov between `B` and `A` (and thus error on `B` redefining `x` to `str`), because it ensures that the structural type `B` is assignable to the structural type `A`. This isn't _necessary_, but I think it matches the usual intuition for what inheritance means.

I'm also not sure why the `@final` on `B` actually matters here; I think the arguments for and against requiring `B` to match `A` are the same whether or not `B` is final?

---

_Comment by @jorenham on 2025-03-21 16:54_

> > Note that the LSP does not apply to `Protocol` that's marked as `@final`
> 
> If I understand correctly, you are describing a case like this?
> 
> from typing import Protocol, final
> 
> class A(Protocol):
>     x: int
> 
> @final
> class B(A, Protocol):
>     x: str
> In this case it does seem intuitive to me to enforce Liskov between `B` and `A` (and thus error on `B` redefining `x` to `str`), because it ensures that the structural type `B` is assignable to the structural type `A`. This isn't _necessary_, but I think it matches the usual intuition for what inheritance means.
> 
> I'm also not sure why the `@final` on `B` actually matters here; I think the arguments for and against requiring `B` to match `A` are the same whether or not `B` is final?

Hmm, this example shows that it isn't as easy as I initially thought, so I'll try to explain a bit better what I'm trying to say here. 

One of the use-cases I had in mind, is this one

```py
@final
class Unhashable(Protocol):
    __hash__: ClassVar[None]
```

If you'd enforce LSP here, then the `__hash__` definition would be flagged as an LSP violation. But because this protocol 1) cannot be instantiated, and 2) cannot be subclassed, I was thinking that this LSP violation will never lead to type-safe unsafe situation. 

But your example shows that these 2 conditions are not sufficient to guarantee type-safety in the absence of LSP &mdash; an additional condition is required.

---

So how about this: 

*The LSP does not apply to a method or attribute of a final `Protocol` when it directly overrides a member of `object`, and only of `object`.*

Another way to think about it, is that final protocols behave as if they don't have `object` as a base class.

---

To illustrate, let's squash our examples, and apply the updated conditions:

```py
class A(Protocol):
    x: int

@final
class UnhashableAndHasX(A, Protocol):
    __hash__: ClassVar[None]  # LSP doesn't apply => accept
    x: str                    # LSP applies       => reject
```

Does that make sense?

---

_Comment by @carljm on 2025-03-21 17:17_

In principle, every Python object is an instance of `object`, and the API surface of`object` must be true for every Python object.

Thus, in principle, a protocol that isn't Liskov compatible with `object` is useless and describes a type that has no inhabitants (equivalent to `Never`).

However, as we know, for pragmatic/historical reasons, unfortunately the description of `object` in typeshed claims some things to be true which are not actually true for all Python objects, and causes Liskov violations in some situations.

But IMO this means that this is not an issue related specifically to Protocol or structural typing (because, for the _accurate_ parts of the typeshed `object` definition, there is no value in, or need for, any special case for structural types). Thus I wouldn't support a general rule about Protocols and Liskov compatibility with `object`, because if we assumed a fully accurate typeshed `object`, that would be a useless / nonsensical rule.

Rather, this is an issue of how we handle Liskov for a few particular attributes of `object` (like `__hash__`) that are described wrongly in typeshed. I think the problem is specific to those problem attributes, but is not specific to Protocol; it is just as much a problem for non-Protocol inheritance as for Protocol inheritance.

---

_Renamed from "[red-knot] enforce Liskov on subclasses" to "enforce Liskov on subclasses" by @MichaReiser on 2025-05-07 15:26_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:53_

---

_Renamed from "enforce Liskov on subclasses" to "enforce Liskov Substitution Principle on subclasses" by @AlexWaygood on 2025-08-13 15:47_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 13:09_

---

_Comment by @vlashada on 2025-11-17 19:51_

What is the status on this? Would be really useful to have!

---

_Comment by @AlexWaygood on 2025-11-18 18:21_

> What is the status on this? Would be really useful to have!

The first PR for this is now up: https://github.com/astral-sh/ruff/pull/21436

---

_Comment by @vlashada on 2025-11-26 19:10_

Thank you for starting to enforce Liskov Substitution Principle! It does catch many cases, but it still does not error if I forget to specify the return type for a method on a subclass:

```python
class A:
    def greet(self) -> str:
        return "hello from A"

class B(A):
    def greet(self) -> int:  # error: invalid-method-override (as expected)
        return 123

class C(A):
    def greet(self):  # this line does not have an error
        return 123
```

---

_Comment by @carljm on 2025-11-26 19:16_

@vlashada This checking should come along with return-type inference, see #128. Mypy doesn't do RTI (or catch this unsafe override), pyright does.

---

_Comment by @AlexWaygood on 2025-11-26 19:17_

@vlashada our Liskov checks are working as intended here. Currently, we view any function lacking an explicit return-type annotation as implicitly returning `Any`/`Unknown` (the two concepts are equivalent), and `Any`/`Unknown` are assignable to all types, including `str`. This may change if https://github.com/astral-sh/ty/issues/128 is implemented, but until then you can prevent this unsoundness by enforcing [Ruff's `ANN` rules](https://docs.astral.sh/ruff/rules/#flake8-annotations-ann) on your code base 

---

_Removed from milestone `Beta` by @MichaReiser on 2025-12-05 15:50_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-05 15:50_

---

_Comment by @Paul-B98 on 2025-12-06 20:17_

Hey, I'm not sure if I'm in the right place or if I've misunderstood, but could someone advise me on how to handle the following example?

```python
class A:
    pass

class B(A):
    pass

class C:
    def test(self, state: A) -> None:
        pass

class D(C):
    def test(self, state: B) -> None:  # Invalid override of method `test`: Definition is incompatible with `C.test` 
        pass
```

---

_Comment by @carljm on 2025-12-06 20:49_

Hi @Paul-B98 ,

This is an example of unsafe code that this check is intended to catch. If D is a subtype of C, that means anywhere you annotate as C it should be safe to pass a D. But if C's method accepts A and D's method accepts only B, that's not safe: code that thinks it has a C (but actually has a D at runtime) might try to pass an A to that method, which will blow up as D is only expecting instances of B as argument to that method.

In general, when you override a method, the parameters of the subclass version need to accept everything the base class method accepts (or more -- but not less). So it's safe to have the base class accept B and the subclass accept A, but not the other way around.

So your options here are either to write your code differently, or if you are ok with the potential unsafety, use a `# ty: ignore` comment, or globally disable the invalid method override check. 

---

_Comment by @Paul-B98 on 2025-12-06 21:51_

Hey @carljm, thanks for the clarification. How would this change if C were to become a protocol or an abstract base class (with an abstract method)? Would it make sense to have the option to create a minimal type for that "interface", or should I simply remove the type and define it in the subclasses?

---

_Comment by @carljm on 2025-12-08 18:00_

@Paul-B98 The core issue here doesn't change at all if C is a protocol or abstract base class. For the type `C` to be safely usable as an annotation, all subtypes of `C` that override `test` must override it with a signature that is a subtype of the signature defined by `C.test`, which requires that the signature can accept any calls accepted by the signature of `C.test`.

I'm not sure what you mean by "option to create a minimal type for that interface." You do have that option, the problem is that if you define the minimal interface as "has a method `test` which accepts an instance of `A`", then a type which has a method `test` which does not accept any instance of `A`, is simply not a type which implements that interface: `D` in your example above does not implement the interface of `C`.

So yes, if you want `D.test` to only accept `B` instances, and a hypothetical `E.test` to only accept `B2` instances (where `E` is a subclass of `C` or implements the `C` interface and `B2` is another subclass of `A`), then the only way to make that type-safe is to remove the `test` method from the interface defined by `C` entirely. That eliminates the problem, because it means that code using an instance `c` of type `C` won't be able to unsafely call `c.test(...)` blithely assuming it can pass any `A` as argument.

---

_Comment by @AlexWaygood on 2025-12-08 18:09_

@Paul-B98, one option you might consider is to make your abstract base class generic, e.g.

```py
class A:
    pass

class B(A):
    pass

class Abstract[T: A]:
    def test(self, state: T) -> None:
        pass

class Concrete(Abstract[B]):
    def test(self, state: B) -> None:
        pass
```

Or, if you're using Python <3.12:

```py
from typing import TypeVar, Generic

T = TypeVar("T", bound="A")

class A:
    pass

class B(A):
    pass

class Abstract(Generic[T]):
    def test(self, state: T) -> None:
        pass

class Concrete(Abstract[B]):
    def test(self, state: B) -> None:
        pass
```

All major type checkers agree that this does not violate the Liskov Substitution Principle.

---

_Comment by @Paul-B98 on 2025-12-08 18:24_

Thanks for the comments.

---

_Renamed from "enforce Liskov Substitution Principle on subclasses" to "Tracking issue for override-related checks (including theh Liskov Substitution Principle)" by @AlexWaygood on 2025-12-22 13:02_

---

_Renamed from "Tracking issue for override-related checks (including theh Liskov Substitution Principle)" to "Tracking issue for override-related checks (including the Liskov Substitution Principle)" by @AlexWaygood on 2025-12-22 13:02_

---
