```yaml
number: 1927
title: "Emit diagnostic on unsound call to abstract `@classmethod` or `@staticmethod` with trivial body, when accessed on the class object itself"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-12-16T13:31:49Z
updated_at: 2025-12-16T20:34:53Z
url: https://github.com/astral-sh/ty/issues/1927
synced_at: 2026-01-12T15:54:26Z
```

# Emit diagnostic on unsound call to abstract `@classmethod` or `@staticmethod` with trivial body, when accessed on the class object itself

---

_@AlexWaygood_

Both [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=GYJw9gtgBAhgRgYygSwgBzCALrOBnLEGBLCAUywAswATAKDoQBsY88oAxALjqj6gACzVnnJVavfgPgEiJMdXr8oNMsCgLaACmZ4AlFAC0APhQA7LFygA6WwxBkAbmRhMA%2BlgCeaMlo7XNGi09PTogA) and [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEq2AxnRDcbpQC4PZxeVUTLjRhcAFrkwAddLKZRUcOHQBiiWXS10AAgqVxREqZu07G-QcKOSZ6bXUwwwdG1IAUCuAEo6AWgA%2BVnQuRDpCCNlZShgANxhUKAB9LlJiGHdVQjdMd29vWRAAGhAAVy5oOBJyRBAAYjoAVQqoCFS6MFL0YQhcdDgorGcOzhpULiT0UppsGEp3fDCIEN9AuksNe20YrlLKezBpEAA5adnKMOB8AF8jwpKyGLAoUkIuWigKBoAFUieX9YYHAEOhMPqQADme3GvXQhFkDQAyjAYHRxFwuMQ4IgAPQ4x7OF6ETgQnEwdA4zC4JhwHFg9CQ6EVPo4kaUBixVDQRiwUHgiBQwTM%2By4YjCqqyMjGdB%2BeKUOCwugAXjoRwAzIQAIwAJju6BA1xKQgq8VU0BgFDQWDwRDIBqAA) detect the unsoundness here, though [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=cf7dbf1c58f50ff7fb4528abfceed39d) does not. We should also detect this as unsound and reject it:

```py
from abc import abstractmethod

class F:
    @classmethod
    @abstractmethod
    def method(cls) -> int: ...

# pyright: Method "method" cannot be called because it is abstract and unimplemented  (reportAbstractUsage)
reveal_type(F.method())
```

Pyright and pyrefly do however allow the classmethod to be called via `type[F]`. You could argue that this is unsound, since you do not know whether or not you're dealing with a concrete subclass of `F` or not. But `type[]` types are generally unsound, and mypy's lint attempting to enforce similar soundness checks for `type[]` types has proved [very unpopular](https://github.com/python/mypy/issues/4717). So I think we should also allow this:

```py
from abc import abstractmethod

class F:
    @classmethod
    @abstractmethod
    def method(cls) -> int: ...

def _(x: type[F]):
    reveal_type(x.method())
```

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-16 13:31_

---

_Label `typing semantics` added by @AlexWaygood on 2025-12-16 13:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-16 17:45_

---

_Comment by @AlexWaygood on 2025-12-16 20:23_

Note that instance methods don't need to be caught by this check, because if we implement https://github.com/astral-sh/ty/issues/1877 then it's basically impossible to call an abstract instance method.

This check should probably also include abstract staticmethods with trivial bodies, though, thinking about it 

---

_Comment by @carljm on 2025-12-16 20:25_

But isn't this kind of thing a valid use case for abstract types and methods?

```py
# A is a type with abstract methods

def f(a: A):
    a.some_abstract_method()
```

where the whole point is that you want callers to pass you concrete subtypes of `A`?

---

_Comment by @AlexWaygood on 2025-12-16 20:30_

Yes, I just said that we shouldn't include instance methods in this check, only classmethods and maybe staticmethods. And above that, I also said that we should only emit the lint for abstract classmethods/staticmethods with trivial bodies accessed on the class object itself, not when accessed on `type[]` types.

---

_Renamed from "Emit diagnostic on unsound call to abstract `@classmethod` with trivial body" to "Emit diagnostic on unsound call to abstract `@classmethod` or `@staticmethod` with trivial body, when accessed on the class object itself" by @AlexWaygood on 2025-12-16 20:34_

---
