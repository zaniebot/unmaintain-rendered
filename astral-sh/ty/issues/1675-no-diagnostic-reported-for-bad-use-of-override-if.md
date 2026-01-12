```yaml
number: 1675
title: "No diagnostic reported for bad use of `@override` if the method has other decorators too"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-11-29T15:38:23Z
updated_at: 2025-12-01T23:20:06Z
url: https://github.com/astral-sh/ty/issues/1675
synced_at: 2026-01-12T15:54:25Z
```

# No diagnostic reported for bad use of `@override` if the method has other decorators too

---

_@AlexWaygood_

Similar to #1674. We do not currently issue a complaint about this snippet, but pyre (the predecessor to pyrefly, and the reference implementation for PEP 698) [does](https://pyre-check.org/play?input=from%20typing%20import%20override%0Afrom%20functools%20import%20lru_cache%0A%0Aclass%20Bar%3A%0A%20%20%20%20%40override%0A%20%20%20%20%40lru_cache%0A%20%20%20%20def%20method(self%2C%20x%3A%20int)%20-%3E%20None%3A%20...):

```py
from typing import override
from functools import lru_cache

class Bar:
    @override
    @lru_cache
    def method(self, x: int) -> None: ...
```

The reason why we issue no complaint is that the `lru_cache` decorator transforms the function definition into an instance of `_lru_cache_wrapper`. We currently only recognise methods as being `@override` if they have `Type::FunctionLiteral` types in our internal model, so the transformation into an instance of `_lru_cache_wrapper` means that we "forget" that the method was decorated with `@override`. To fix this issue, we may need to treat the `@override` decorator more like a type qualifier that "survives" the transformations from a function-literal type to a callable type and then to an `_lru_cache_wrapper` type.

Mypy [also issues a diagnostic](https://mypy-play.net/?mypy=latest&python=3.12&gist=4cae2098f63f2cc2a361bf8ea6da0cae) on this snippet. At time of writing, neither [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.12&reportUnreachable=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMpgBuApiCEgCbEBQokUwArigMYxhgA2Azptrvk4hGAfRYBDFgAsa1Fp3HdeAIXEgAXNSjaoAASKlyVLTt1DRE6TR1QqwKBGIwpYCgApuxTsAA0UAB7qmCgwAJRQALQAfFAAcmAoxEEAdKnUQA) nor [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN2R0qKAB10YavTABXdAGMGuXFDiduvflErSA%2BnNRyAFjDFi5UVHFUAxJYjF1HdAAJbd%2BoyfROXg4Q6dMGDA6GhgGQ1xMAAo4GCgwABo6fEROdAYASjoAWgA%2BOgA5XHQYNMIK03kLKzoAIVRKaNtcTPtvJ1dtPQNjAMcgkLCIqNj4pJS01izcguLS8sr0EESQaQZoOBJyRBAAYjoAVQ2oCCYBWQUIErgqwYFeGlQGHXRpGmwYJtT0mfy6OAMSjtHyUcLSSjeMAiECFd6fYF0YD4AC%2BMLEKxAZDBYCgpEIihoUAoBwACqQcXiARgcAQ6HISpA2BDntd0IQxAcAMowGB0QwMBjEOCIAD0ouxwTxhF4bFFMHQoswuDkcFFDPEEGZlFZJVFD0odFQADdUNBUNhYPTGVqWRsSnRcMR7egtmIyCN0DljV84Gy6ABeOgwgDMhAAjAAmdHLFGrAwbH3WaAwChoLB4IhkEAooA) do, however. Note that pyright has an [open issue from a user](https://github.com/microsoft/pyright/issues/11130) that requests this feature.

---

_Added to milestone `Stable` by @AlexWaygood on 2025-11-29 15:38_

---

_Label `typing semantics` added by @AlexWaygood on 2025-11-29 15:38_

---

_Comment by @carljm on 2025-12-01 23:17_

I don't think this is necessary to address before stable release?

---

_Comment by @AlexWaygood on 2025-12-01 23:20_

OK

---

_Removed from milestone `Stable` by @AlexWaygood on 2025-12-01 23:20_

---
