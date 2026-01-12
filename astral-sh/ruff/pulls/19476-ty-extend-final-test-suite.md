```yaml
number: 19476
title: "[ty] Extend `Final` test suite"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/extend-final-tests
created_at: 2025-07-22T07:04:22Z
updated_at: 2025-07-22T10:06:49Z
url: https://github.com/astral-sh/ruff/pull/19476
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Extend `Final` test suite

---

_@sharkdp_

## Summary

Restructures and cleans up the `typing.Final` test suite. Also adds a few more tests with TODOs based on the [typing spec for `typing.Final`](https://typing.python.org/en/latest/spec/qualifiers.html#uppercase-final).




---

_Label `testing` added by @sharkdp on 2025-07-22 07:04_

---

_Review requested from @carljm by @sharkdp on 2025-07-22 07:04_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-22 07:04_

---

_Review requested from @dcreager by @sharkdp on 2025-07-22 07:04_

---

_Label `ty` added by @sharkdp on 2025-07-22 07:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:280 on 2025-07-22 09:19_

we could also add tests for these, both of which should be illegal:

```py
from typing import Final

class Foo(Final[tuple[int]]): ...

reveal_type(Foo.__mro__)

def f() -> Final[None]: ...
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:296 on 2025-07-22 09:21_

how does `__new__` play into things here? (I can't remember what the spec says.) E.g. this feels ~fine to me... but if the spec disallows it, then the spec disallows it, I guess:

```py
from typing import Final

class Foo:
    def __new__(cls):
        self = object.__new__(cls)
        self.x: Final = 42
        return self
```

it's basically the pattern that `fractions.Fraction` uses in the stdlib: https://github.com/python/cpython/blob/9a21df7c0a494e2819775eabd522ebec994d96c0/Lib/fractions.py#L205-L311

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:306 on 2025-07-22 09:22_

I think it would be clearer here to use a different name for the loop variable -- the declaration in the loop body is illegal because the variable would be repeatedly assigned on each iteration of the loop, not because it shadows the loop variable:

```suggestion
for _ in range(10):
    # TODO: This should be an error
    i: Final[int] = 1
```

---

_@AlexWaygood approved on 2025-07-22 09:22_

---

_@sharkdp reviewed on 2025-07-22 09:23_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:306 on 2025-07-22 09:23_

Oops, yes!

---

_@sharkdp reviewed on 2025-07-22 09:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:296 on 2025-07-22 09:57_

The spec says (emphasis mine):

> Finally, as `self.id: Final = 1` (also optionally with a type in square brackets). **This is allowed only in `__init__` methods**, so that the final instance attribute is assigned only once when an instance is created.

In your code snippet in particular, any declaration would be illegal in that position. You can't annotate attribute assignments if they don't refer to the first parameter in a method (ty does not yet enforce this, see https://github.com/astral-sh/ty/issues/509). 

---

_@AlexWaygood reviewed on 2025-07-22 10:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:296 on 2025-07-22 10:06_

hmm, I think there might be value in special-casing `__new__` methods specifically so that we allow attribute annotations in `__new__` methods where the value of the attribute expression is an instance of `cls`. But we can defer that discussion to a later PR

---

_Merged by @sharkdp on 2025-07-22 10:06_

---

_Closed by @sharkdp on 2025-07-22 10:06_

---

_Branch deleted on 2025-07-22 10:06_

---
