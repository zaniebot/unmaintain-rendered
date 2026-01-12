```yaml
number: 2170
title: "Synthesize a precise signature for `NamedTuple` `_replace` methods"
type: issue
state: closed
author: AlexWaygood
labels:
  - namedtuples
assignees: []
created_at: 2025-12-22T19:31:34Z
updated_at: 2025-12-23T21:33:56Z
url: https://github.com/astral-sh/ty/issues/2170
synced_at: 2026-01-12T15:54:26Z
```

# Synthesize a precise signature for `NamedTuple` `_replace` methods

---

_@AlexWaygood_

The `._replace()` call here fails at runtime, because this particular `NamedTuple` only supports an `x` keyword argument being passed to its `_replace` method:

```py
from typing import NamedTuple

class X(NamedTuple):
    x: int

x = X(42)
x._replace(y=5)
```

Mypy [catches this error](https://mypy-play.net/?mypy=latest&python=3.12&gist=8f9140c6be68df5eb09199a6d5f9d122), though pyright and pyrefly don't at the time of writing.

Note that despite the leading underscore, the `_replace` method of namedtuple classes [_is_ public](https://docs.python.org/3/library/collections.html#collections.somenamedtuple._replace). The leading underscore is there so that the generated method doesn't clash with any fields a user might want to have on their `NamedTuple` class.

---

_Label `namedtuples` added by @AlexWaygood on 2025-12-22 19:31_

---

_Comment by @AlexWaygood on 2025-12-22 19:34_

Mypy also appears to synthesize a precise type for the `_fields` attribute ([also public](https://docs.python.org/3/library/collections.html#collections.somenamedtuple._fields)). Or at least, more precise than what we infer right now:

```py
from typing import NamedTuple

class X(NamedTuple):
    x: int
    y: str

reveal_type(X._fields)  # mypy: `tuple[str, str]`; ty/pyright/pyrefly: `tuple[str, ...]`

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-23 00:25_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-12-23 00:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-23 00:40_

---

_Comment by @carljm on 2025-12-23 01:44_

Opened https://github.com/astral-sh/ty/issues/2176 for `_fields` since the PR that will close this issue only handles `_replace` (and I think it's fine to separate them.)

---

_Closed by @charliermarsh on 2025-12-23 21:33_

---
