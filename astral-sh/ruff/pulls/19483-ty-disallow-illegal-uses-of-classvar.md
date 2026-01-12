```yaml
number: 19483
title: "[ty] Disallow illegal uses of `ClassVar`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/illegal-classvar-use
created_at: 2025-07-22T11:42:54Z
updated_at: 2025-07-22T12:21:31Z
url: https://github.com/astral-sh/ruff/pull/19483
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Disallow illegal uses of `ClassVar`

---

_@sharkdp_

## Summary

It was faster to implement this then to write the ticket: Disallow `ClassVar` annotations almost everywhere outside of class body scopes.

## Test Plan

New Markdown tests

---

_Review requested from @carljm by @sharkdp on 2025-07-22 11:42_

---

_Label `ty` added by @sharkdp on 2025-07-22 11:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-22 11:42_

---

_Review requested from @dcreager by @sharkdp on 2025-07-22 11:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:105 on 2025-07-22 11:45_

Does the spec explicitly forbid something like this?

```py
class C:
    @classmethod
    def f(cls):
        cls.x: ClassVar[int] = 1
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:129 on 2025-07-22 11:46_

```suggestion
# TODO: Show `Unknown` instead of `@Todo` type in the MRO; or ignore `ClassVar` and show the MRO as if `ClassVar` was not there
```

---

_Comment by @github-actions[bot] on 2025-07-22 11:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-07-22 11:47_

---

_@sharkdp reviewed on 2025-07-22 11:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:105 on 2025-07-22 11:55_

> Does the spec explicitly forbid something like this?

It does not explicitly forbid it. It [does say](https://typing.python.org/en/latest/spec/class-compat.html#classvar) *"`ClassVar` may be used in one of several forms:"* and then proceeds to show two examples where `ClassVar` is used at the class-body level.

Mypy, pyright and pyrefly all reject your example for various reasons (including: "ClassVar" is not allowed in this context).

I also [didn't find any references](https://grep.app/search?regexp=true&q=%5C.%5Ba-z%5D%2B%3A+ClassVar) of code like this in the ecosystem.

---

_@AlexWaygood reviewed on 2025-07-22 12:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:105 on 2025-07-22 12:01_

I think it would be reasonable to support that, but I also don't think it's high priority at all, especially if no other type checker does.

---

_@sharkdp reviewed on 2025-07-22 12:06_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:105 on 2025-07-22 12:06_

By "support that", you mean: "tolerate that"? Because I'm not sure what the intention here would be? When would a `cls.x: … = 1` attribute not be a (pure) class variable?

---

_@AlexWaygood reviewed on 2025-07-22 12:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:105 on 2025-07-22 12:12_

I guess if it were declared with `x: int = 42` in the class body, then that would make it an instance attribute with a class-level default, and `cls.x = 56` in a class method would be mutating the default value of the instance attribute? That's a bit suspect, though; I'd consider it at least a code smell.

And I suppose we'd want to emit an error if it were declared as an instance variable in an instance method or the class body, but declared as a class variable in a classmethod

---

_Merged by @sharkdp on 2025-07-22 12:21_

---

_Closed by @sharkdp on 2025-07-22 12:21_

---

_Branch deleted on 2025-07-22 12:21_

---
