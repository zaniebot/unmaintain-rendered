```yaml
number: 15815
title: False negative for PYI016 Duplicate union member
type: issue
state: closed
author: nickdrozd
labels:
  - question
assignees: []
created_at: 2025-01-29T18:50:20Z
updated_at: 2025-02-02T18:53:29Z
url: https://github.com/astral-sh/ruff/issues/15815
synced_at: 2026-01-12T15:54:55Z
```

# False negative for PYI016 Duplicate union member

---

_@nickdrozd_

### Description

```python
Q = int | int
```

Not flagged, should raise PYI016

ruff 0.9.3

---

_Comment by @AlexWaygood on 2025-01-29 19:07_

This is because Ruff can't be sure here that `Q` is a type alias, so it doesn't know whether to analyse it as a type expression or a value expression. If you explicitly annotate it with `typing.TypeAlias` or `typing_extensions.TypeAlias`, the diagnostic is emitted as expected:

```py
from typing import TypeAlias

Q: TypeAlias = int | int
```

Same if you use a `type` statement on Python 3.12 or newer:

```py
type Q = int | int
```

---

_Label `question` added by @AlexWaygood on 2025-01-29 19:07_

---

_Comment by @nickdrozd on 2025-01-30 18:43_

> This is because Ruff can't be sure here that Q is a type alias, so it doesn't know whether to analyse it as a type expression or a value expression.

I don't think it matters how it is used. AFAICT, there is literally no difference between `int | int` and `int`:

```python
assert int is int | int
assert int == int | int

##############################

Q = int | int

assert Q is int

q = int

assert q is q | q

assert Q is q

##############################

from typing import TypeAlias

P: TypeAlias = int | int

assert P is int

assert P is Q

assert P is q
```

Is there any situation in which `int | int` would be preferred to `int`? Or IOW, any situation in which flagging this would be a false positive?

Can't speak for others, but I have definitely done this before, and it was definitely a mistake.

---

_Comment by @AlexWaygood on 2025-01-30 21:26_

From a human's perspective, it's obvious that `X = int | int` is an attempt to create a type alias. But from a static-analysis tool's perspective, how can it unambiguously differentiate `X = int | int` from `Y = 42 | 56` (which is not a type alias, just a normal value being assigned to a variable)?

This would be _easier_ for Ruff if it had better type inference. But even for type checkers, differentiating type aliases from other assignments isn't _easy_. If it _were_ easy, there wouldn't be any need for `typing.TypeAlias` at all, and [PEP 613](https://peps.python.org/pep-0613/) would never have been written :-)

---

_Closed by @MichaReiser on 2025-02-02 18:53_

---
