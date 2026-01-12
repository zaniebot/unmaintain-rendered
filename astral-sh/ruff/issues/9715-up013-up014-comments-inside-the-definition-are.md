```yaml
number: 9715
title: "UP013, UP014: Comments inside the definition are dropped"
type: issue
state: open
author: JelleZijlstra
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2024-01-30T19:44:22Z
updated_at: 2024-12-08T19:08:22Z
url: https://github.com/astral-sh/ruff/issues/9715
synced_at: 2026-01-12T15:54:49Z
```

# UP013, UP014: Comments inside the definition are dropped

---

_@JelleZijlstra_

Starting with this code:

```python
from typing import NamedTuple, TypedDict

X = TypedDict("X", {
    "some_config": int,  # important
})

Y = NamedTuple("Y", [
    ("some_config", int),  # important
])
```

Ruff (via rules UP013 and UP014) turns it into:

```python
from typing import NamedTuple, TypedDict

class X(TypedDict):
    some_config: int

class Y(NamedTuple):
    some_config: int
```

Ideally it should retain the end-of-line comments.


---

_Label `autofix` added by @AlexWaygood on 2024-01-30 20:42_

---

_Label `bug` added by @AlexWaygood on 2024-01-30 20:43_

---

_Comment by @chris-griffin on 2024-12-08 18:29_

Ruff has recently updated their policy to clarify that rule [fixes that drop comments should be marked as unsafe](https://github.com/astral-sh/ruff/pull/14300/files).

---

_Label `help wanted` added by @charliermarsh on 2024-12-08 18:31_

---

_Comment by @AlexWaygood on 2024-12-08 19:08_

The fix is now marked as unsafe if comments will be dropped, but ideally (as Jelle says) we wouldn't drop them at all in the autofix.

---
