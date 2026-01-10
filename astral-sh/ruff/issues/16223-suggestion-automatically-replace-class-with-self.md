```yaml
number: 16223
title: "[Suggestion] Automatically replace \"Class\" with Self from typing"
type: issue
state: open
author: montanarograziano
labels:
  - rule
assignees: []
created_at: 2025-02-18T10:15:52Z
updated_at: 2025-04-25T15:03:22Z
url: https://github.com/astral-sh/ruff/issues/16223
synced_at: 2026-01-10T11:09:57Z
```

# [Suggestion] Automatically replace "Class" with Self from typing

---

_Issue opened by @montanarograziano on 2025-02-18 10:15_

### Description

I'm in the process of migrating a codebase from python 3.9 to 3.12. Doing so I have some functions type hinted with the class name in brackets, as typing.Self was not available in py 3.9.
An example:
```py
class Test:
    def __init__(self) -> None:
        self.a = "Hello World"

    def test(self) -> "Test":
        return self
   
```
Could it be useful to have a fix for "Class" annotation to be converted to typing.Self if project's python version >= 3.11? I was checking on the doc if there was already a rule for this scenario but I haven't found it.
Thanks in advance for your help!

---

_Comment by @InSyncWithFoo on 2025-02-18 10:55_

There is no rule for this currently. Rightfully so, arguably, since `-> Class` and `-> Self` have different semantics ([Mypy](https://mypy-play.net/?mypy=1.15.0&python=3.13&flags=strict&gist=4c3a515aee0ff24888591a5a30379653), [Pyright](https://pyright-play.net/?pyrightVersion=1.1.394&pythonVersion=3.14&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDKApgDbAA0UYAbkSCEgCZECwAUO%2BwMYkCGAzvygBhAFzsAkM2BR%2BvCEQD6PAfwAU-UsACUUALQA%2BEaKgA6c5OmytGrbsOEtJ86c5sVgqABE1w7eLYJAAEaOgZmSyIZOQVlPkFbMnsjMUkJECIYAFcQFBE1XSgAYihgVFYOQJDaeiYKqSjrMkSdfSNiMgCJdMycvOECqGKoMNwTAANhccwhFDB8VSQ0FF4AIxIiWDAocY7gcbd2DNpeEkV4BCJfAtMYpQ91bUKS4SOiE7OLq59tW-l7%2BKPZ4iNzHIinc6IK4DX6aZpPIaIxEvN4fSGXNQ-W42BFI5HedhAA)):

```python
class C:
	def same_class(self) -> C: ...
	def self(self) -> Self: ...

class D(C):
	@override
	def same_class(self) -> C:
		return C()  # fine

	@override
	def self(self) -> Self:
		return C()  # error: `C` is not assignable to `Self`
```

```python
reveal_type(C().same_class())  # C
reveal_type(D().same_class())  # C

reveal_type(C().self())        # C
reveal_type(D().self())        # D
```

---

_Comment by @AlexWaygood on 2025-02-18 12:01_

@InSyncWithFoo is correct. We shouldn't have a rule to automatically make this replacement in _all_ cases, because the old type annotation and the new one mean very different things.

@montanarograziano, the type annotation for your `Test.test` method was unfortunately subtly incorrect on Python 3.9. The method always returns `self`, so if you called it on a subclass of `Test`, it wouldn't return an instance of `Test` _exactly_; it would return an instance of the subclass of `Test`. The correct annotation for the method on Python 3.9 would be

```py
from typing import TypeVar

T = TypeVar("T", bound="Test")

class Test:
    def __init__(self) -> None:
        self.a = "Hello World"

    def test(self: T) -> T:
        return self
```

If your `Test` method was correctly annotated, then our https://docs.astral.sh/ruff/rules/custom-type-var-for-self/ rule would automatically upgrade the annotations to use `Self` rather than a custom TypeVar, as the custom TypeVar _is_ exactly equivalent to `Self` here.

What we could do, however, is to add a correctness rule that, on _all_ Python versions, detects functions that only ever return `self` and tells you that the return annotation is likely incorrect if it hardcodes the name of the class rather than using `Self` or a custom TypeVar. (On Python versions where `Self` is available, it could have an autofix to use `Self`, but doing that would likely be hard on older Python versions where you have to use a custom TypeVar.) We have a similar rule as https://docs.astral.sh/ruff/rules/non-self-return-type/, but that's less comprehensive (it just hardcodes knowledge about certain methods that nearly always return `self`, rather than actually analysing the body of the method).

---

_Label `rule` added by @AlexWaygood on 2025-02-18 12:01_

---

_Comment by @montanarograziano on 2025-02-18 12:07_

Thanks for the great explanation, didn't know about the TypeVar thing!


---

_Comment by @jack-mcivor on 2025-04-25 15:03_

> What we could do, however, is to add a correctness rule that, on all Python versions, detects functions that only ever return self and tells you that the return annotation is likely incorrect if it hardcodes the name of the class rather than using Self

I think this would be a really helpful rule. @AlexWaygood should this be a separate feature request or can we re-use this issue? As well as `return self`, I quite often see classmethods that do `return cls(...)` which should similarly use a Self return annotation.

> We have a similar rule as https://docs.astral.sh/ruff/rules/non-self-return-type/, but that's less comprehensive (it just hardcodes knowledge about certain methods that nearly always return self, rather than actually analysing the body of the method).

FWIW I was confused by the documentation for this rule -- I initially expected the example given to trigger the rule, which it doesn't. However, the docs *do* go on to explain that only specific methods are caught. Anyway, the example is:
```python
class Shape:
    def set_scale(self, scale: float) -> Shape:
        self.scale = scale
        return self

class Circle(Shape):
    def set_radius(self, radius: float) -> Circle:
        self.radius = radius
        return self

# Type checker infers return type as `Shape`, not `Circle`.
Circle().set_scale(0.5)

# Thus, this expression is invalid, as `Shape` has no attribute `set_radius`.
Circle().set_scale(0.5).set_radius(2.7)
```

---
