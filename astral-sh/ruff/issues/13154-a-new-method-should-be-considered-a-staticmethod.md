```yaml
number: 13154
title: "A `__new__` method should be considered a staticmethod, not a classmethod."
type: issue
state: closed
author: cake-monotone
labels:
  - bug
assignees: []
created_at: 2024-08-29T18:40:56Z
updated_at: 2025-02-16T20:12:27Z
url: https://github.com/astral-sh/ruff/issues/13154
synced_at: 2026-01-12T15:54:52Z
```

# A `__new__` method should be considered a staticmethod, not a classmethod.

---

_@cake-monotone_

This is a subtle tweak: The `__new__` method is currently causing classmethod warnings (e.g., ARG003). However, it should actually be causing staticmethod warnings (e.g., ARG004).

https://play.ruff.rs/204c57b4-a35e-48d5-8f92-ca2e109dd2cd

```python
class A:
    def __new__(cls, unused_static_method_arguments):
        return 42


A.__new__(A, 1)  # It works!!  If __new__ is treated as a class method, it should be `A.__new__(1)`
```


NOTE: According to the official Python documentation, the `__new__` method is a staticmethod that takes the class itself (cls) as its first argument.

### Reference

> Called to create a new instance of class cls. [`__new__`()](https://docs.python.org/3/reference/datamodel.html#object.__new__) is a static method (special-cased so you need not declare it as such) that takes the class of which an instance was requested as its first argument.

https://docs.python.org/3/reference/datamodel.html#object.__new__


---

_Comment by @AlexWaygood on 2024-08-30 11:02_

Thanks for opening the issue! There's some pretty interesting questions here.

`__new__` defies normal classification. On the one hand, you're absolutely correct -- `__new__` is a static method at runtime, not a classmethod, and this is true whether or not it's explicitly decorated with `@staticmethod`. On the other hand, it _behaves_ much more like a classmethod than a staticmethod because of how heavily it's special-cased by the interpreter. In your example here, it's true that the `cls` argument is strictly-speaking unused, and so perhaps it would be strictly-speaking correct to emit two `ARG004` diagnostics on the `__new__` method there. But in practice, the vast majority of the time `__new__` is called implicitly rather than explicitly -- and in those situations, you _do_ need to have at least one `__new__` parameter, even if it's unused, or instantiating your class isn't going to work. In that sense, it's much more like a classmethod than a staticmethod:

```pycon
>>> class A:
...     def __new__():
...         return object.__new__(A)
... 
>>> A()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: A.__new__() takes 0 positional arguments but 1 was given
```

Regardless of whether we think ARG003 or ARG004 should apply to `__new__` methods, though, we should definitely fix the discrepancy here in the following snippet. `A` and `B` have identical semantics at runtime, but one `ARG003` diagnostic is emitted for `A`, while two `ARG004` diagnostics are emitted for `B`:

```py
class A:
    def __new__(cls, unused):
        return 42


class B:
    @staticmethod
    def __new__(cls, unused):
        return 42
```

https://play.ruff.rs/57d4f602-c444-4b54-9f68-fe21215582b4

---

_Label `bug` added by @AlexWaygood on 2024-08-30 11:02_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-30 11:32_

---

_Comment by @AlexWaygood on 2024-08-30 12:04_

Hmm, I'm not sure what the right answer here is:
- I think whether a `__new__` method is explicitly decorated with `@staticmethod` or not, it should probably have the same error code for the diagnostic, because it will have the same runtime semantics
- I think even though a `__new__` method is technically a staticmethod at runtime, it behaves more like a classmethod, so it makes sense to apply "classmethod rules" when considering whether it's necessary for the method to have at least one argument or not. That implies that we should apply `ARG003` to `__new__` methods, even if they're decorated with `@staticmethod`
- But the message emitted by `ARG003` is "Unused class method argument", and I think it could be really confusing for users if that message is emitted on a method explicitly decorated with `@staticmethod`

```py
class A:
    def __new__(cls, unused):
        return 42


class B:
    @staticmethod
    def __new__(cls, unused):
        return 42
```

@carljm can I ping you for a second opinion? I wonder if `__new__` methods should just be their own rule entirely. It seems like we already have quite different rule classifications to the original `flake8-unused-arguments` flake8 plugin.

---

_Unassigned @AlexWaygood by @AlexWaygood on 2024-08-30 12:04_

---

_Comment by @carljm on 2024-08-30 20:47_

I think we should treat `__new__` as a static method, and the error code and message should not reference classmethods; referencing a classmethod when `__new__` is not a classmethod is just too confusing/wrong (regardless of whether there is an explicit `@staticmethod` decorator on it or not.)

I could go either way on whether we should complain about an unused first argument on `__new__`. On the one hand, it's probably a mistake to have an unused first argument, even though the first argument is required. On the other hand, that logic would also suggest that the classmethod rule should also still complain about unused first argument, too -- it's generally a mistake to have a classmethod that doesn't use the first argument, it should be a staticmethod instead.

If we prefer not to ever emit ARG003 or ARG004 for an argument that is required to be present, I don't see any problem with using the ARG004 error code for `__new__`, but also excluding the first argument from it as a special case. I think that even though that makes `__new__` handling somewhat unique, it's much better than emitting any code mentioning classmethods for `__new__`.

---

_Comment by @cake-monotone on 2024-09-03 01:32_

I also agree with carljm's opinion.

For the first time, like AlexWaygood, I thought this issue was quite subtle.

https://github.com/astral-sh/ruff/blob/3463683632b0af2457f4449f7daef77d5527cd9c/crates/ruff_python_semantic/src/analyze/function_type.rs#L28-L37

However, `__new__` is internally being treated as a classmethod. Considering that new rules applied to staticmethods might not be applied to `__new__` in the future, I think `__new__` should be treated as a staticmethod.

---

_Renamed from "A `__new__` method should be considered a 'static method', not a 'class method'." to "A `__new__` method should be considered a staticmethod, not a classmethod." by @cake-monotone on 2024-09-03 01:34_

---

_Closed by @dylwil3 on 2025-02-16 20:12_

---

_Closed by @dylwil3 on 2025-02-16 20:12_

---
