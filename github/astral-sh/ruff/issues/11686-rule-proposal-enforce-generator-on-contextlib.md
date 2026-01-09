---
number: 11686
title: "Rule Proposal: Enforce `generator` on `contextlib.contextmanager` decorated functions"
type: issue
state: open
author: max-muoto
labels:
  - rule
assignees: []
created_at: 2024-06-02T01:39:19Z
updated_at: 2024-07-26T01:26:29Z
url: https://github.com/astral-sh/ruff/issues/11686
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule Proposal: Enforce `generator` on `contextlib.contextmanager` decorated functions

---

_Issue opened by @max-muoto on 2024-06-02 01:39_

In the past, it was fairly common for people to simply put `Iterator` on context managers simply out of convenience, as typing out `Generator`, required adding in two (mostly useless) generic params. However, this can lead to runtime failures not caught by a type-checker, take this example from https://github.com/python/typeshed/issues/2772.

```python
import contextlib
from typing import Iterator

@contextlib.contextmanager
def f() -> Iterator[int]:
    return iter([1])


with f():
    pass


with f():
    raise TypeError('wat')
```

Due to how breaking this change is, the typeshed maintainers were are reluctant to change the underlying annotation to require a `Generator`. It was actually proposed offloading this responsibility to linting tules, or special rules within the type-checkers themselves.

Now, `Iterator[int]` can be simplified to `Generator[int]` with the addition of type var defaults, and their usage in the cpython implementation, and the typesheds, see: https://github.com/python/typeshed/pull/11867

I would propose adding a rule to autofix `Iterator[T]` to `Generator[T]` on 3.13+ for any functions decorated with `contextlib.contextmanager`, or when deferred annotations are present in the file, as otherwise a runtime error will be encountered. In cases where neither are true, an error can simply be flagged and fixed manually (either through quoting `Generator[T]`, or doing `Generator[T, None, None]`). 

This will help ensure that type-checkers such as Pyright and MyPy can flag and catch issues that might be hidden as a result.
 

---

_Renamed from "Rule Proposal: Enforce `generator` on `contextlib.contextmanager` decorated function" to "Rule Proposal: Enforce `generator` on `contextlib.contextmanager` decorated functions" by @max-muoto on 2024-06-02 01:39_

---

_Label `rule` added by @charliermarsh on 2024-07-26 01:26_

---
