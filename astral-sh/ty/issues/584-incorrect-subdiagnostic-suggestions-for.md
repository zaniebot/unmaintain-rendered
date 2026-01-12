```yaml
number: 584
title: "Incorrect subdiagnostic suggestions for `unresolved-reference` diagnostics"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-06-05T11:04:24Z
updated_at: 2025-06-27T12:40:34Z
url: https://github.com/astral-sh/ty/issues/584
synced_at: 2026-01-12T15:54:23Z
```

# Incorrect subdiagnostic suggestions for `unresolved-reference` diagnostics

---

_@AlexWaygood_

### Summary

https://github.com/astral-sh/ruff/pull/18444 added fantastic subdiagnostic suggestions for `unresolved-reference` diagnostics in methods. They work great for most cases, but for several edge cases they result in incorrect suggestions.

---

We should not add the subdiagnostic if the method is a staticmethod: the subdiagnostic does not make much sense here:

```py
class Foo:
    def __init__(self):
        self.x = 42

    @staticmethod
    def static_method():
        print(x)
```

Our diagnostic is currently:

```
error[unresolved-reference]: Name `x` used when not defined
  --> foo.py:9:15
   |
 7 |     @staticmethod
 8 |     def static_method():
 9 |         print(x)
   |               ^
10 |
11 |     @classmethod
   |
info: An attribute `x` is available: consider using `self.x`
info: rule `unresolved-reference` is enabled by default
```

---

We should not suggest attributes only available on instances if the method is a classmethod: the subdiagnostic does not make much sense here:

```py
class Foo:
    def __init__(self):
        self.x = 42

    @classmethod
    def class_method(cls):
        print(x)
```

Our diagnostic is currently:

```
error[unresolved-reference]: Name `x` used when not defined
 --> bar.py:7:15
  |
5 |     @classmethod
6 |     def class_method(cls):
7 |         print(x)
  |               ^
  |
info: An attribute `x` is available: consider using `self.x`
info: rule `unresolved-reference` is enabled by default
```

This should be fixed by using `SubclassOf::from(self.db(), class.default_specialization(self.db()))` [here](https://github.com/astral-sh/ruff/blob/8485dbb324212dab0e26d2afb5929097af129bbf/crates/ty_python_semantic/src/types/infer.rs#L6060) if it's a classmethod (but still using `Type::instance(self.db(), class.default_specialization(self.db()))` if it's an instance method).

---

The subdiagnostic message always says "consider using `self.<attribute>`". But we should inspect the function to check what the name of the first parameter is rather than assuming it's `self`. Even for instance methods, the name of the first parameter is often not `self` if it's an instance method on a metaclass, for example:

```py
class Foo:
    Y = 42

    @classmethod
    def class_method(cls):
        print(Y)


class Meta(type):
    Z = 42
    
    def instance_metaclass_method(cls):
        print(Z)
```

Our current diagnostics:

```
error[unresolved-reference]: Name `Y` used when not defined
 --> baz.py:6:15
  |
4 |     @classmethod
5 |     def class_method(cls):
6 |         print(Y)
  |               ^
  |
info: An attribute `Y` is available: consider using `self.Y`
info: rule `unresolved-reference` is enabled by default

error[unresolved-reference]: Name `Z` used when not defined
  --> baz.py:13:15
   |
12 |     def instance_metaclass_method(cls):
13 |         print(Z)
   |               ^
   |
info: An attribute `Z` is available: consider using `self.Z`
info: rule `unresolved-reference` is enabled by default
```

---

_Label `bug` added by @AlexWaygood on 2025-06-05 11:04_

---

_Label `help wanted` added by @AlexWaygood on 2025-06-05 11:04_

---

_Label `diagnostics` added by @AlexWaygood on 2025-06-05 11:04_

---

_Comment by @MatthewMckee4 on 2025-06-05 21:05_

Is there a plan to introduce KnownClass::Staticmethod? It seems like it may be useful for this

---

_Comment by @AlexWaygood on 2025-06-05 21:20_

Huh, I assumed that we already had a `STATIC_METHOD` flag on this bitflag, but I guess not! I'd suggest adding it and detecting staticmethods in the same way that we detect classmethods

https://github.com/astral-sh/ruff/blob/5faf72a4d9b50c6e330165685e57fae14ca68b73/crates/ty_python_semantic/src/types/function.rs#L93

---

_Comment by @MatthewMckee4 on 2025-06-05 21:23_

Okay thanks, ill have a go at refactoring to use that

---

_Comment by @MatthewMckee4 on 2025-06-05 21:26_

I think detecting staticmethods still requires KnownClass::Staticmethod. I believe this is how we add the bitflag

```rs
Type::ClassLiteral(class) => {
    if class.is_known(self.db(), KnownClass::Classmethod) {
        function_decorators |= FunctionDecorators::CLASSMETHOD;
        continue;
    }
}
```

---

_Comment by @AlexWaygood on 2025-06-05 21:32_

That's fine!

---

_Comment by @MatthewMckee4 on 2025-06-05 21:33_

I guess this can be added in another PR? Or should i add it here?

---

_Comment by @carljm on 2025-06-05 23:21_

> I guess this can be added in another PR? Or should i add it here?

Seems fine to add it in the same PR, it's a simple addition following existing patterns exactly

---

_Closed by @AlexWaygood on 2025-06-27 12:40_

---
