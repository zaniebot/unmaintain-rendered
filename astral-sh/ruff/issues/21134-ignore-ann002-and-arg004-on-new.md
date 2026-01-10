```yaml
number: 21134
title: Ignore ANN002 and ARG004 on __new__
type: issue
state: open
author: NorthLake
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-30T09:11:17Z
updated_at: 2025-11-21T09:43:00Z
url: https://github.com/astral-sh/ruff/issues/21134
synced_at: 2026-01-10T11:10:00Z
```

# Ignore ANN002 and ARG004 on __new__

---

_Issue opened by @NorthLake on 2025-10-30 09:11_

### Summary

The special method `__new__` requires its signature (specifically the amount of [kw]args) to be the same as the signature of the `__init__` method. This means that some of the arguments on the `__new__` method may not be used, while they are still required. Therefore, it would make sense to ignore ARG004 (unused-static-method-argument) on the `__new__` method. Or better yet, check wether its signature corresponds to the signature of `__init__`.

Secondly, because the parameters on `__new__` may not be used, it makes sense to declare the method as `def __new__(cls, *args, **kwargs)` and only specify the specific parameters on the `__init__` method. This is why it would also make sense to me if ANN002 (missing-type-args) is ignored in this case.

Sample code:
```python
class ThisClass:
    def __new__(cls, *args, **kwargs) -> "ThisClass":  # raises ARG004 and ANN002
        return super().__new__(cls)

    def __init__(self, some_argument: str) -> None:
        self.some_argument = some_argument
```

---

_Comment by @ntBre on 2025-10-30 18:54_

This sounds plausible to me, but in your example, shouldn't `*args` and `**kwargs` be passed along in the `super()__new__` call? Currently they appear to be genuinely unused. That only covers ARG004, though.

I'm curious what others think too. cc @amyreese 

---

_Label `rule` added by @ntBre on 2025-10-30 18:54_

---

_Label `needs-decision` added by @ntBre on 2025-10-30 18:54_

---

_Comment by @NorthLake on 2025-10-30 19:16_

> This sounds plausible to me, but in your example, shouldn't `*args` and `**kwargs` be passed along in the `super()__new__` call? Currently they appear to be genuinely unused.

Strangely that is not the case, as per [Python's datamodel reference](https://docs.python.org/3/reference/datamodel.html#object.__new__):

> If `__new__()`  is invoked during object construction and it returns an instance of cls, then the new instance’s `__init__` method will be invoked like `__init__(self[, ...])`, where self is the new instance and the remaining arguments are the same as were passed to the object constructor.

As you can see, `__init__` is automatically called if an instance of the class in question is returned. This means that the signature of a class's `__new__` method should be the same as its `__init__` method. I now realize that it would be even better if the signature of `__new__` is compared with `__init__` and a warning is given if the signatures do not match. Not sure if this is possible or easy, though.

On the other hand, there is a special case:

> If `__new__()` does not return an instance of cls, then the new instance’s `__init__()` method will not be invoked.

This means that `__new__` could technically have a different signature if you don't plan on actually returning an instance of the class in question. However, in my opinion this sounds really smelly, so I wouldn't bother checking for this.


> That only covers ARG004, though.

That's true. As I said, you could check if the two signatures are the exact same. However, you could also argue for having a single source of truth, and not warning for a generic untyped `*args, **kwargs` signature. In hindsight, checking whether the signatures are the same sounds better than just not typing `__new__`, but it is an alternative to consider.

---

_Comment by @amyreese on 2025-10-30 19:57_

> This sounds plausible to me, but in your example, shouldn't `*args` and `**kwargs` be passed along in the `super()__new__` call? Currently they appear to be genuinely unused. That only covers ARG004, though.
> 
> I'm curious what others think too. cc [@amyreese](https://github.com/amyreese)

More to the point, passing anything besides `cls` to `object.__new__()` results in a type error:

```
>>> class Foo:
...     def __new__(cls, *args, **kwargs):
...         print(cls, args, kwargs)
...         super().__new__(cls, *args, **kwargs)
...
>>> f = Foo()
<class '__main__.Foo'> () {}
>>> f = Foo(value=1)
<class '__main__.Foo'> () {'value': 1}
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    f = Foo(value=1)
  File "<python-input-4>", line 4, in __new__
    super().__new__(cls, *args, **kwargs)
    ~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^
TypeError: object.__new__() takes exactly one argument (the type to instantiate)
```

It's also unlikely that you consume all, or even any, of the arguments in `__new__`, so I do think it makes sense to ignore unused/unannotated `*args`/`**kwargs` specifically in `__new__`, but *not* ignore unused variables if they were given a name.

---

_Comment by @NorthLake on 2025-10-30 20:43_

> More to the point, passing anything besides `cls` to `object.__new__()` results in a type error:

This is only true for `object.__new__`. If `class Foo` inherits from `class Bar`, and `Bar.__init__` expects some arguments, then `Bar.__new__` should also include those parameters. Therefore, `Foo.__new__` should call `Bar.__new__` with the additional necessary arguments.

```python
class Bar:
    def __new__(cls, arg1):
        print("Bar.__new__:", arg1)
        return super().__new__(cls)

    def __init__(self, arg1):
        print("Bar.__init__:", arg1)

class Foo(Bar):
    def __new__(cls):
        print("Foo.__new__")
        return super().__new__(cls, "hello")

    def __init__(self):
        print("Foo.__init__")
        super().__init__("world")

Foo()
```
Gives the following output:
```
Foo.__new__
Bar.__new__: hello
Foo.__init__
Bar.__init__: world
```

And that's as much as I can think right now, as it's getting pretty late... Maybe tomorrow I'll be able to think about this some more.

---

_Comment by @NorthLake on 2025-10-30 21:06_

I now realize that this:

> If `__new__()` does not return an instance of cls, then the new instance’s `__init__()` method will not be invoked.

might actually have to do with inheritance as well. If the returned object is of a subtype (and therefore not actually an instance of cls itself), then `__init__` is not called. In that case it is up to the subtype to call `super().__init__` from it's own `__init__` method.

Not sure about this, though.

---
