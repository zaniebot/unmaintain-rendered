```yaml
number: 2375
title: Callable type has no attribute __name__
type: issue
state: closed
author: lervag
labels:
  - question
  - callables
assignees: []
created_at: 2026-01-07T11:50:21Z
updated_at: 2026-01-07T12:51:55Z
url: https://github.com/astral-sh/ty/issues/2375
synced_at: 2026-01-10T01:56:41Z
```

# Callable type has no attribute __name__

---

_Issue opened by @lervag on 2026-01-07 11:50_

### Summary

Consider this short snippet:

```python
from concurrent.futures import Future, ProcessPoolExecutor
from typing import Callable, ParamSpec, TypeVar

P = ParamSpec("P")
T = TypeVar("T")


class ProcessPoolManager:
    def __init__(self, max_workers: int):
        self.max_workers: int = max_workers
        self.__executor = ProcessPoolExecutor(max_workers=self.max_workers)

    def submit_job(self, fn: Callable[P, T], *args: P.args, **kwargs: P.kwargs) -> Future[T]:
        future = self._executor.submit(fn, *args, **kwargs)
        future.add_done_callback(lambda f: self._handle_job_completion(f, fn.__name__))

        return future
```

With pyright/basedpyright, I don't get any type warnings (it seems the assume callable is a named function). But with `ty` I get the following:

```
> ty check test.py
error[unresolved-attribute]: Object of type `(**P@submit_job) -> T@submit_job` has no attribute `__name__`
  --> test.py:15:75
   |
13 |     def submit_job(self, fn: Callable[P, T], *args: P.args, **kwargs: P.kwargs) -> Future[T]:
14 |         future = self._executor.submit(fn, *args, **kwargs)
15 |         future.add_done_callback(lambda f: self._handle_job_completion(f, fn.__name__))
   |                                                                           ^^^^^^^^^^^
16 |
17 |         return future
   |
help: Function objects have a `__name__` attribute, but not all callable objects are functions
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

The help message is very useful here, and I see that it is probably right. But I can't figure out how to either change my signature to only allow "named" functions or how to adjust so that we only get `__name__` if it is available. I was hoping someone here might have an idea?

### Version

```sh
> ty version
ty 0.0.9
```

---

_Comment by @AlexWaygood on 2026-01-07 12:16_

Hey, thanks for the report -- nice writeup :-) does this FAQ help answer your questions? <https://docs.astral.sh/ty/reference/typing-faq/#why-does-ty-say-callable-has-no-attribute-__name__>

---

_Label `question` added by @AlexWaygood on 2026-01-07 12:16_

---

_Label `callables` added by @AlexWaygood on 2026-01-07 12:16_

---

_Comment by @AlexWaygood on 2026-01-07 12:17_

hmm, maybe we should just link to the FAQ directly from our diagnostic?

---

_Comment by @lervag on 2026-01-07 12:38_

Thanks! That was definitely helpful!

After posting my issue I ended up solving my issue like this:

```python
    def submit_job(self, fn: Callable[P, T], *args: P.args, **kwargs: P.kwargs) -> Future[T]:
        future = self._executor.submit(fn, *args, **kwargs)

        fn_name = getattr(fn, "__name__", "unnamed")
        if not isinstance(fn_name, str):
            fn_name = "unnamed"

        future.add_done_callback(lambda f: self._handle_job_completion(f, fn_name))
        return future
```

That resolves my typing issue. I don't think we'd ever run into a case where my original variant would fail, but I don't see a reason not to just stay on the safe side.

Again thanks for the quick reply and for already having written a good explenation of this issue!

---

_Closed by @lervag on 2026-01-07 12:38_

---

_Comment by @lervag on 2026-01-07 12:51_

Oh, I missed the info box on my first read:

```python
if TYPE_CHECKING:
    from ty_extensions import Intersection

    type FunctionLikeCallable[**P, R] = Intersection[Callable[P, R], FunctionType]
else:
    FunctionLikeCallable = Callable
```

That's neat and exactly what I wanted. <3

---
