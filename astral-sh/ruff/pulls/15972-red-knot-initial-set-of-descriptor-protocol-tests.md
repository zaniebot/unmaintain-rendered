```yaml
number: 15972
title: "[red-knot] Initial set of descriptor protocol tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/initial-descriptor-protocol-tests
created_at: 2025-02-05T14:52:00Z
updated_at: 2025-02-05T18:49:39Z
url: https://github.com/astral-sh/ruff/pull/15972
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Initial set of descriptor protocol tests

---

_Pull request opened by @sharkdp on 2025-02-05 14:52_

## Summary

This is a first step towards creating a test suite for [descriptors](https://docs.python.org/3/howto/descriptor.html). It does not (yet) aim to be exhaustive.

relevant ticket: #15966 

## Test Plan

Compared desired behavior with the runtime behavior and the behavior of existing type checkers.


---

_Label `testing` added by @sharkdp on 2025-02-05 14:52_

---

_Label `red-knot` added by @sharkdp on 2025-02-05 14:52_

---

_Review requested from @carljm by @sharkdp on 2025-02-05 14:52_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-05 14:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-05 14:52_

---

_@AlexWaygood approved on 2025-02-05 15:18_

Nice. Something else you could add (but doesn't _need_ to be this PR) is descriptors that have different set-types to their get-types:

```py
class Foo:
    def __init__(self):
        self._value: int | None = None

    def __get__(self) -> int | None:
        return self._value

    def __set__(self, val: int | str) -> None:
        self._value = int(val)

class Bar:
    f = Foo()

b = Bar()
reveal_type(b.f)  # revealed: int | None
b.f = "42"  # okay!
b.f = None  # error!
reveal_type(b.f)  # revealed: int | None
```

---

_Comment by @mishamsk on 2025-02-05 16:42_

I was wondering if it would make sense to include the two most common implicit descriptors:

````md
## Built-in property descriptor

The built-in `property` decorator creates a descriptor. It is implemented in Python, but behaves the same as custom descriptor. It should correctly infer return and setter types:

```python
class C:
    _name: str | None = None
    
    @property
    def name(self) -> str:
        return self. _name or "Unset"
        
    @name.setter
    def name(self, value: str | None) -> None:
        self._value = value

c = C()
reveal_type(c._name)  # revealed: str | None
reveal_type(c.name)  # revealed: str
reveal_type(C.name)  # TODO: should be some virtual `builtins.property` instance

c.name = "new"  # okay!
c.name = 42  # error!
```

## Built-in classmethod descriptor

Similarly to `property`, `classmethod` decorator creates an implicit descriptor that binds the first argument to the class instead of the instance. Type inference should work for both class and instance access:

```python
class C:
    def __init__(self, value: str) -> None:
          self._name: str = value

    @classmethod
    def factory(cls, value: str) -> "C":
        return cls(value)
        
    @classmethod
    def get_name(cls) -> str:
        return cls.__name__

reveal_type(C.factory)  # TODO: should be some virtual `builtins.classmethod` instance
reveal_type(C("42").factory)  # TODO: should be some virtual `builtins.classmethod` instance

c1 = C.factory("test")  # okay
reveal_type(c1)  # revealed: C
reveal_type(C.get_name())  # revealed: str
reveal_type(C("42").get_name())  # revealed: str
```
````

---

_Comment by @AlexWaygood on 2025-02-05 16:52_

> I was wondering if it would make sense to include the two most common implicit descriptors:

These look like great tests to add! I think we'll probably have enough tests for both `classmethod` and `property` in due course that it would make sense to devote a whole mdtest suite to each. I'll leave it to @sharkdp whether he wants to add those new files in this PR or not :-)

> ```
> reveal_type(C.name)  # TODO: should be some virtual `builtins.property` instance
> ```

Hmm, not sure what you mean by "virtual" here -- I think this should just be a normal `builtins.property` instance?

> ```
> reveal_type(C.factory)  # TODO: should be some virtual `builtins.classmethod` instance
> reveal_type(C("42").factory)  # TODO: should be some virtual `builtins.classmethod` instance
> ```

This isn't _quite_ correct, unfortunately. The `factory` object stored in `C`'s `__dict__` here is an instance of `builtins.classmethod`. But if you access it on the class or the instance, you get a `types.MethodType` instance, not a `classmethod` instance (due to the descriptor protocol):

```pycon
>>> class Foo:
...     @classmethod
...     def bar(cls): ...
...     
>>> Foo.__dict__['bar']
<classmethod(<function Foo.bar at 0x1057ae2a0>)>
>>> isinstance(Foo.__dict__['bar'], classmethod)
True
>>> Foo.bar
<bound method Foo.bar of <class '__main__.Foo'>>
>>> import types
>>> isinstance(Foo.bar, types.MethodType)
True
>>> isinstance(Foo.bar, classmethod)
False
```

---

_Comment by @mishamsk on 2025-02-05 17:20_

@AlexWaygood sorry, my bad, rushed this one in case @sharkdp decided to merge early. Was writing from my head. Thanks a lot for pointing out my mistakes!

> Hmm, not sure what you mean by "virtual" here -- I think this should just be a normal builtins.property instance?

I remember that `property` is not implemented in pure Python, so I wasn't sure how the return object class is called. Thanks for correcting!

Same for the other comment. Let me correct myself:

````md
## Built-in property descriptor

The built-in `property` decorator creates a descriptor. It should correctly infer return and setter types:

```python
class C:
    _name: str | None = None
    
    @property
    def name(self) -> str:
        return self. _name or "Unset"
        
    @name.setter
    def name(self, value: str | None) -> None:
        self._value = value

c = C()
reveal_type(c._name)  # revealed: str | None
reveal_type(c.name)  # revealed: str
reveal_type(C.name)  # revealed: `builtins.property`

c.name = "new"  # okay!
c.name = 42  # error!
```

## Built-in classmethod descriptor

Similarly to `property`, `classmethod` decorator creates an implicit descriptor that binds the first argument to the class instead of the instance.

```python
class C:
    def __init__(self, value: str) -> None:
          self._name: str = value

    @classmethod
    def factory(cls, value: str) -> "C":
        return cls(value)
        
    @classmethod
    def get_name(cls) -> str:
        return cls.__name__

c1 = C.factory("test")  # okay
reveal_type(c1)  # revealed: C
reveal_type(C.get_name())  # revealed: str
reveal_type(C("42").get_name())  # revealed: str
```
````

I think adding the test for `__dict__` access is a bit too much for initial tests. There are also `__set_name__` that can theoretically do some magic with initialization and `__delete__` is not covered. But I'll elaborate on that in the #15966 

---

_Comment by @AlexWaygood on 2025-02-05 17:29_

> > Hmm, not sure what you mean by "virtual" here -- I think this should just be a normal builtins.property instance?
> 
> I remember that `property` is not implemented in pure Python, so I wasn't sure how the return object class is called. Thanks for correcting!

Yes, `property` is implemented in C, but it's still a class, and it still has instances just like any other class. Python's object model is thankfully very consistent on this point (nowadays, at least -- it wasn't always so in the early days of Python!).

> I think adding the test for `__dict__` access is a bit too much for initial tests.

Oh, absolutely! I'm not suggesting that we should add that to our tests. That was just an example to help demonstrate that there _is_ a `classmethod` instance _somewhere_ -- it's just not what is returned when the class method is accessed, regardless of whether the class method is accessed on the instance or the class

---

_Merged by @sharkdp on 2025-02-05 18:47_

---

_Closed by @sharkdp on 2025-02-05 18:47_

---

_Branch deleted on 2025-02-05 18:47_

---

_Comment by @sharkdp on 2025-02-05 18:49_

> rushed this one in case @sharkdp decided to merge early

@mishamsk Thank you for your contribution! I am merging this, but the whole idea here was just to set up an initial batch of tests that we could later extend. Please feel free to send changes/extensions to this document if/when you want to work on descriptors.

---
