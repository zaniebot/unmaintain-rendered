```yaml
number: 1646
title: "Improve diagnostics to explain why a `TypedDict` is not assignable to `dict`"
type: issue
state: open
author: lypwig
labels:
  - diagnostics
  - typeddict
assignees: []
created_at: 2025-11-26T15:45:26Z
updated_at: 2025-11-26T17:05:45Z
url: https://github.com/astral-sh/ty/issues/1646
synced_at: 2026-01-12T15:54:25Z
```

# Improve diagnostics to explain why a `TypedDict` is not assignable to `dict`

---

_@lypwig_

### Question

I use a package containing methods returning poorly typed dict, and I would like to improve typing in my inherited classes by defining a typedDict and set it as a return type:

```py
from typing import Any, TypedDict


class FooDict(TypedDict):
    a: int


class A:
    def foo(self) -> dict[str, Any]:
        return {"a": 1}


class B(A):
    def foo(self) -> FooDict: #  <-- ty error here
        return {"a": 1}
```

Here I get:

> ty: Invalid override of method `foo`: Definition is incompatible with `A.foo` (invalid-method-override) [Ln 14, Col 9]

https://play.ty.dev/6d88c3d3-7801-46ce-98f8-8d74a0e7fce4

Is it possible to do this?

### Version

ty 0.0.1-alpha.28

---

_Label `question` added by @lypwig on 2025-11-26 15:45_

---

_Comment by @AlexWaygood on 2025-11-26 16:18_

Hi @njourdane. Unfortunately no, this isn't possible. A TypedDict type is not usually assignable to `dict[str, Any]`. The reasons for this are laid out in https://typing.python.org/en/latest/spec/typeddict.html#subtyping-with-dict. (The spec _does_ list one or two exemptions to this rule... but unfortunately, we haven't implemented those exemptions yet 🙃. It's on our roadmap to do so.)

A `TypedDict` type _is_ a subtype of `Mapping[str, Any]`, but I assume this is not an option for you if you don't control the package that `A` is defined in...?

Your best option here might be to either `type: ignore` away the error or to use `typing.cast`.

---

_Label `typeddict` added by @AlexWaygood on 2025-11-26 16:18_

---

_Renamed from "How to surcharge TypedDicts?" to "Improve diagnostics to explain why a `TypedDict` is not assignable to `dict`" by @AlexWaygood on 2025-11-26 16:59_

---

_Label `question` removed by @AlexWaygood on 2025-11-26 16:59_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-26 16:59_

---

_Comment by @AlexWaygood on 2025-11-26 17:00_

I think ideally we would give a subdiagnostic that would explain why `TypedDict` types cannot be assigned to `dict`. That's a bit hard for us to do right now, but we plan on improving our architecture in the future so that we can surface these kinds of explanatory subdiagnostics when subtyping/assignability checks fail.

---

_Comment by @carljm on 2025-11-26 17:05_

> The spec _does_ list one or two exemptions to this rule

Those exemptions can roughly be summarized as "if your `TypedDict` type provides zero type information beyond what a `dict` type would provide, then you can assign it to `dict`" -- in other words, they are pretty much useless in practice. (But we should still implement them at some point.)

---
