```yaml
number: 2176
title: "Synthesize precise type for `_fields` of a NamedTuple"
type: issue
state: closed
author: carljm
labels:
  - namedtuples
assignees: []
created_at: 2025-12-23T01:43:49Z
updated_at: 2025-12-23T21:44:54Z
url: https://github.com/astral-sh/ty/issues/2176
synced_at: 2026-01-12T15:54:26Z
```

# Synthesize precise type for `_fields` of a NamedTuple

---

_@carljm_

> Mypy also appears to synthesize a precise type for the `_fields` attribute ([also public](https://docs.python.org/3/library/collections.html#collections.somenamedtuple._fields)). Or at least, more precise than what we infer right now:
> 
> ```py
> from typing import NamedTuple
> 
> class X(NamedTuple):
>     x: int
>     y: str
> 
> reveal_type(X._fields)  # mypy: `tuple[str, str]`; ty/pyright/pyrefly: `tuple[str, ...]` 

 _Originally posted by @AlexWaygood in [#2170](https://github.com/astral-sh/ty/issues/2170#issuecomment-3683861456)_

---

_Renamed from "Synthesize precise type for `_fields_ of a NamedTuple" to "Synthesize precise type for `_fields` of a NamedTuple" by @AlexWaygood on 2025-12-23 08:02_

---

_Label `namedtuples` added by @AlexWaygood on 2025-12-23 11:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-23 13:46_

---

_Comment by @AlexWaygood on 2025-12-23 21:44_

Fixed in https://github.com/astral-sh/ruff/pull/22163

---

_Closed by @AlexWaygood on 2025-12-23 21:44_

---

_Comment by @charliermarsh on 2025-12-23 21:44_

(Oof, thanks, fixed the PR summary.)

---
