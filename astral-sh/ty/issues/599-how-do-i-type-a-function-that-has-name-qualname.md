```yaml
number: 599
title: "how do i type a function that has `__name__`/`__qualname__` attributes?"
type: issue
state: closed
author: DetachHead
labels:
  - question
assignees: []
created_at: 2025-06-07T07:25:38Z
updated_at: 2025-11-13T06:31:33Z
url: https://github.com/astral-sh/ty/issues/599
synced_at: 2026-01-10T02:06:24Z
```

# how do i type a function that has `__name__`/`__qualname__` attributes?

---

_Issue opened by @DetachHead on 2025-06-07 07:25_

### Question

i see that ty fixes the issue that other type checkers have where all `Callable` types are assumed to have a `__name__` attribute:

```py
from typing import Callable

def foo(): ...

foo.__name__ # no error

def _(fn: Callable[[], None]):
    fn.__name__ # error
```

i know this is the correct behavior, but i'm now encountering false positives on decorators that turn a function into a `Callable` type:

```py
from contextlib import contextmanager

@contextmanager
def foo(): ...

foo.__name__ #     Type `(...) -> _GeneratorContextManager[Unknown, None, None]` has no attribute `__name__` (unresolved-attribute)
```

what's ty's plan to address issues like this? for example will there be a `ty_extensions.FunctionType` type that can be used to denote a function that has these attributes?

### Version

0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Label `question` added by @DetachHead on 2025-06-07 07:25_

---

_Renamed from "how do i type a function that has a `__name__`/`__qualname__` attributes?" to "how do i type a function that has `__name__`/`__qualname__` attributes?" by @DetachHead on 2025-06-07 07:27_

---

_Comment by @AlexWaygood on 2025-06-07 08:46_

For now, you could create a custom protocol that has a `__call__` method and a `__name__` attribute:

```py
from typing import Protocol

class FunctionLike[**P, R](Protocol):
    def __call__(self, *args: P.args, **kwargs: P.kwargs) -> R: ...
    __name__: str
```

Or you could possibly use `ty_extensions.Intersection[types.FunctionType, Callable[[], None]]`?

> what's ty's plan to address issues like this?

Honestly: we're not yet sure! There's so much code that relies on these `types.FunctionType` attributes existing on `Callable` types, that we may have to copy what pyright and mypy do here, even though I agree that it's less sound. If we end up doing that, I think we'd like to provide a sound alternative to `Callable` that is not assumed to have all the attributes and behaviours that `types.FunctionType` instances have.

---

_Comment by @DetachHead on 2025-06-07 09:07_

> Honestly: we're not yet sure! There's so much code that relies on these `types.FunctionType` attributes existing on `Callable` types, that we may have to copy what pyright and mypy do here, even though I agree that it's less sound. If we end up doing that, I think we'd like to provide a sound alternative to `Callable` that is not assumed to have all the attributes and behaviours that `types.FunctionType` instances have.

that sounds like a good idea, as long as we have the option to ban usage of the old unsafe `Callable` type.

honestly it's just a relief that there's finally a type checker where the maintainers are open minded to actually try to solve some of these difficult issues. i've been playing around with ty for a few hours and i've noticed it's already got some great new diagnostics that found issues in my project which went undetected by both mypy and pyright.

everything you guys have released so far has been amazing and i'm already very impressed with ty despite how unfinished it is. you guys are single-handedly fixing the python ecosystem 🚀🚀🚀

---

_Comment by @KotlinIsland on 2025-06-07 09:09_

related #600

---

_Comment by @benedikt-bartscher on 2025-06-08 12:59_

@DetachHead there is not much to add to your words above, i 100% agree!

Been a user of basedpyright for quite a while now, and I love to see your support and contributions to ty 🚀 

---

_Comment by @carljm on 2025-11-13 06:31_

Going to close this in favor of #1495 which has more recent discussion of the pros and cons of various approaches to this problem. Thanks @DetachHead for raising this issue!

---

_Closed by @carljm on 2025-11-13 06:31_

---
