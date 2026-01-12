```yaml
number: 1674
title: "No diagnostics reported for override of `@final` method if the method has other decorators too"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-11-29T15:28:20Z
updated_at: 2025-12-01T23:26:01Z
url: https://github.com/astral-sh/ty/issues/1674
synced_at: 2026-01-12T15:54:25Z
```

# No diagnostics reported for override of `@final` method if the method has other decorators too

---

_@AlexWaygood_

Similar to #1675. We currently issue no complaint about this snippet, for example, but mypy (the reference implementation for PEP 591) [does](https://mypy-play.net/?mypy=latest&python=3.12&gist=b4d60bd1cba3c19d627d0ff6144aa2c9):

```py
from typing import final
from functools import lru_cache

class Foo:
    @lru_cache
    @final
    def method(self, x: int) -> None: ...

class Bar(Foo):
    @lru_cache
    def method(self, x: int) -> None: ...
```

The reason why we issue no complaint is that the `lru_cache` decorator transforms the function definition into an instance of `_lru_cache_wrapper`. We currently only recognise methods as being `@final` if they have `Type::FunctionLiteral` types in our internal model, so the transformation into an instance of `_lru_cache_wrapper` means that we "forget" that the method was decorated with `@final`. To fix this issue, we may need to treat the `@final` decorator more like a type qualifier that "survives" the transformations from a function-literal type to a callable type and then to an `_lru_cache_wrapper` type.

Pyright [also emits a diagnostic about this override](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.12&reportUnreachable=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMrCoCGANgFCiQECuKAxjGGCQM6ba74kjUD6dROgAsApmTJ0SRFmwBiTAFxkoKqAAFufAcLGr1hFKWWqAJiOBQIImELAmAFCxElgAGigAPBZhQwAlFAAtAB8UAByYCgi3gB0ceKS0mwAQkQg9vJgfkp6Gjz8gqLGKmYWVjZ2js5unt6o-kGhEVGx8UA), but (at time of writing) pyrefly [does not](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN2R0qKAB10YavTABXdAGMGuXFDiduvflErSA%2BnNRyAFjDFi5UVHFUAxJYjF1HdAAJbd%2BoyfROXg4Q6dMGDA6GhgGQ1xMAAo4GCgwABo6fEROdAYASjoAWgA%2BOgA5XHQYNMIK03kLKzoAIVRKaNtcTPtvJ1dtPQNjAMcgkLCIqNj4pJS01izcguLS8sr0EESQaQZoOBJyRBAAYjoAVQ2oCCYBWQUIErgqwYFeGlQGHXRpGmwYJtT0mfy6OAMSjtHyUcLSSjeMAiECFd6fYF0YD4AC%2BMLEKxAZDBYCgpEIihoUAoBwACqQcXiARgcAQ6HISpA2BDntd0IQxAcAMowGB0QwMBjEOCIAD0ouxwTxhF4bFFMHQoswuDkcFFDPEEGZlFZJVFD0odFQADdUNBUNhYPTGVqWRsSnRcMR7egtmIyCN0DljV84Gy6ABeOgwgDMhAAjAAmdHLFGrAwbH3WaAwChoLB4IhkEAooA).

---

_Added to milestone `Stable` by @AlexWaygood on 2025-11-29 15:28_

---

_Label `typing semantics` added by @AlexWaygood on 2025-11-29 15:28_

---

_Comment by @carljm on 2025-12-01 23:26_

Another approach to fixing this kind of issue might be to rely less on the type of the decorated member to preserve this info, and instead attach the info to the type of `Foo` -- that is, have classes keep a set of the names which are final on them?

I'm not sure this needs to be in stable milestone, either, but for now I've given it a P2 and we can consider it later.

---
