```yaml
number: 509
title: Annotated assignments of non-name targets are not checked
type: issue
state: open
author: mtshiba
labels:
  - bug
  - typing semantics
  - attribute access
assignees: []
created_at: 2025-05-25T15:03:54Z
updated_at: 2025-08-15T01:50:31Z
url: https://github.com/astral-sh/ty/issues/509
synced_at: 2026-01-12T15:54:23Z
```

# Annotated assignments of non-name targets are not checked

---

_@mtshiba_

### Summary

The following code passes without any errors.

```python
l = []
l[0]: int = "foo"

class C:
    declared: int

c = C()
c.unresolved: int = "foo"
c.declared: str = "foo"
```

I thought these should be reported as type errors, but it seems that mypy and pyright treat such annotated assignments as "semantically invalid" and report errors without type checking.
In any case, since the current version of ty does not report any problems for this, some kind of fix should be made. In particular, the lack of checking for attribute existence (`c.unresolved`) is an obvious bug that should be fixed.

see also: https://github.com/microsoft/pylance-release/issues/537

### Version

ty 0.0.1a6 (ruff@83a036960)

---

_Label `attribute access` added by @AlexWaygood on 2025-05-25 15:07_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-25 15:08_

---

_Label `bug` added by @sharkdp on 2025-05-26 12:03_

---

_Comment by @carljm on 2025-05-27 17:51_

I think annotated assignments of non-Name targets should be treated as inherently semantically invalid and always error, like mypy and pyright do.

Ideally, in addition to emitting the error that the annotation is invalid, we would just then ignore the annotation, and check the assignment as if it were an un-annotated assignment.

---

_Comment by @jelle-openai on 2025-05-27 18:06_

Note both mypy and pyright accept annotations on attribute assignments of self, like this:

```python
from typing import Any
class X:
    def __init__(self, x: Any):
        self.x: int = x
        
    def method(self) -> None:
        reveal_type(self.x) # int
```

---

_Comment by @carljm on 2025-05-27 18:08_

Yes, thanks: annotated assignments to attributes of `self` should be accepted.

---

_Comment by @sharkdp on 2025-05-28 08:48_

FWIW, pyright also supports this (mypy emits an error on the attribute assignment):
```py
class C:
    @classmethod
    def f(cls) -> None:
        cls.x: int = 1


reveal_type(C.x)  # int
```

---

_Comment by @AlexWaygood on 2025-06-05 10:46_

One related thing that does seem to me to be buggy is that we do not currently infer that `Foo` has an instance attribute `x` for something like this:

```py
class Foo:
    def __init__(self):
        self.x: int

f = Foo()

# Type `Foo` has no attribute `x` (unresolved-attribute)
reveal_type(f.x)  # revealed: Unknown
```

If you're not actually assigning to `self.x`, I don't really know why you'd do this rather than annotating it in the class body, which seems more explicit:

```py
class Foo:
    x: int
```

But [popular libraries such as pytest](https://github.com/pytest-dev/pytest/blob/3e30eae05fcf37880db1db842d022d873cb2166d/src/_pytest/fixtures.py#L399-L407) do this, it appears to be supported by [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=4cd4ceb8f42b1d8342e30df4d50535aa) and [pyright](https://pyright-play.net/?pythonVersion=3.12&strict=true&enableExperimentalFeatures=true&code=MYGwhgzhAEBiD28BcAoa7oBMCmAzaA%2BgQJYB2xALkQBQTYi4CUqGr0dDAdAB5LRkUUKfAF44iaoxQAnbADdsYEAQoBPAA7ZquHoyA), and it doesn't seem any less safe than annotating an instance attribute in the class body without assigning to it. So I think we should add support for this pattern.

Our lack of support for this pattern appears to be the cause of several new false positives in the mypy_primer report in https://github.com/astral-sh/ruff/pull/18041#issuecomment-2888978759.



---

_Comment by @carljm on 2025-06-06 00:18_

> One related thing that does seem to me to be buggy

This is related, but to me seems separate enough from the title of this issue to deserve its own issue. I created https://github.com/astral-sh/ty/issues/590

I suspect this might be pretty easy to fix (and thus could get the "help wanted" label), but I'm not familiar enough with the details of our instance attribute machinery to be confident of that.

---

_Added to milestone `GA` by @carljm on 2025-08-15 01:50_

---
