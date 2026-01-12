```yaml
number: 1684
title: "Detect attempts to override prohibited `NamedTuple` attributes"
type: issue
state: closed
author: AlexWaygood
labels:
  - runtime semantics
  - namedtuples
assignees: []
created_at: 2025-11-29T18:25:40Z
updated_at: 2025-12-02T02:46:59Z
url: https://github.com/astral-sh/ty/issues/1684
synced_at: 2026-01-12T15:54:25Z
```

# Detect attempts to override prohibited `NamedTuple` attributes

---

_@AlexWaygood_

For example:

```pycon
>>> from typing import NamedTuple
>>> class F(NamedTuple):
...     _asdict = 42
...     
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    class F(NamedTuple):
        _asdict = 42
  File "/Users/alexw/Library/Application Support/uv/python/cpython-3.14.0-macos-aarch64-none/lib/python3.14/typing.py", line 3004, in __new__
    raise AttributeError("Cannot overwrite NamedTuple attribute " + key)
AttributeError: Cannot overwrite NamedTuple attribute _asdict
```

This is separate to the check being added in https://github.com/astral-sh/ruff/pull/21697 because `_asdict` here is not considered a `NamedTuple` field either by ty or at runtime (it doesn't have a type annotation). Most assignments in `NamedTuple` class bodies work fine, even if the assigned symbol has a leading underscore, as long as they don't have type annotations:

```pycon
>>> from typing import NamedTuple
>>> class G(NamedTuple):
...     _whatever = 42
...     
>>> 
```

`_asdict`, however, is a [synthesized method](https://docs.python.org/3/library/collections.html#collections.somenamedtuple._asdict) present on all `NamedTuple` classes, and users are not allowed to override it. The full list of prohibited overrides is at https://github.com/python/cpython/blob/77399436bfc87663f9058b749b6cb598bab273f9/Lib/typing.py#L2939-L2942.

Mypy [detects this](https://mypy-play.net/?mypy=latest&python=3.12&gist=a3e1a767632e1843d073a41f2e284a36), though pyright and pyrefly do not at time of writing.

---

_Label `runtime semantics` added by @AlexWaygood on 2025-11-29 18:25_

---

_Label `namedtuples` added by @AlexWaygood on 2025-11-29 18:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-11-30 20:32_

---

_Comment by @AlexWaygood on 2025-11-30 23:42_

(This check should probably be added to overrides.rs)

---

_Closed by @charliermarsh on 2025-12-02 02:46_

---
