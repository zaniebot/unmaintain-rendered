```yaml
number: 2513
title: "Missing diagnostics when parsing `NamedTuple()` and `namedtuple()` calls"
type: issue
state: closed
author: AlexWaygood
labels:
  - runtime semantics
  - typing semantics
  - namedtuples
assignees: []
created_at: 2026-01-15T12:23:11Z
updated_at: 2026-01-15T18:24:27Z
url: https://github.com/astral-sh/ty/issues/2513
synced_at: 2026-01-15T18:49:16Z
```

# Missing diagnostics when parsing `NamedTuple()` and `namedtuple()` calls

---

_@AlexWaygood_

All of the following calls are clearly invalid, but ty currently doesn't emit any diagnostics for them:

```py
import collections
from typing import NamedTuple

Y = collections.namedtuple("Y")

Foo = NamedTuple("Foo")
Bar = NamedTuple("Bar", ["a", "b"])
Baz = NamedTuple("Baz", [("foo", 123), ("bar", 123)])
Spam = NamedTuple("Spam", "definitely not valid")
```

I think for `typing.NamedTuple` we should in fact have _very_ strict parsing. The only reason why you'd use `typing.NamedTuple` over `collections.namedtuple` is because you want precise type inference from your type checker. So while I think we should continue to _not_ emit an error any of these:

```py
from collections import namedtuple

def f(*args, **kwargs):
    B = namedtuple("B", *args)
    C = namedtuple("C", "a b c", *args)
    D = namedtuple(*args, **kwargs)
    E = namedtuple("E", *args, **kwargs)
    F = namedtuple("F", "a b c", *args, **kwargs)
```

I think we should emit an error on all of these (we should ban variadic and keyword-variadic arguments entirely in calls):

```py
from typing import NamedTuple

def f(*args, **kwargs):
    B = NamedTuple("B", *args)
    C = NamedTuple("C", [("a", int), ("b", str)], *args)
    D = NamedTuple(*args, **kwargs)
    E = NamedTuple("E", *args, **kwargs)
    F = NamedTuple("F", [("a", int), ("b", str)], *args, **kwargs)
```


---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-15 12:23_

---

_Assigned to @charliermarsh by @AlexWaygood on 2026-01-15 12:23_

---

_Label `runtime semantics` added by @AlexWaygood on 2026-01-15 12:23_

---

_Label `typing semantics` added by @AlexWaygood on 2026-01-15 12:23_

---

_Label `namedtuples` added by @AlexWaygood on 2026-01-15 12:23_

---

_Closed by @charliermarsh on 2026-01-15 18:24_

---
