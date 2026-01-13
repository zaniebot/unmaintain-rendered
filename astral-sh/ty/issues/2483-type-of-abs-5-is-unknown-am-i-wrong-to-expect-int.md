```yaml
number: 2483
title: "Type of `abs(5)` is `Unknown`.  Am I wrong to expect `int`?"
type: issue
state: closed
author: jpgoldberg
labels:
  - question
assignees: []
created_at: 2026-01-13T22:19:40Z
updated_at: 2026-01-13T22:23:36Z
url: https://github.com/astral-sh/ty/issues/2483
synced_at: 2026-01-13T22:35:50Z
```

# Type of `abs(5)` is `Unknown`.  Am I wrong to expect `int`?

---

_@jpgoldberg_

### Question

ty version 0.0.11 on macOS (arm64) is telling me that the type of `abs(5)` is `Unknown` where I would have expected it to be `int`. 

I am not sure whether this is a bug or "works as designed", as I don't really know what the `@abs` part in the type signature of `abs` means, but I do take 

`def abs[_T](x: SupportsAbs[_T@abs], /) -> _T@abs`.

as suggesting that I should get an `int` when `_T` is an `int`. Also  I note that the type I am seeing for `int.__abs__` is `def __abs__(self: int) -> int`. 

pyright and mypy appear to say `int`. (I haven't checked specific versions, but I only noticed this behavior after switching to ty in VSCode, so the more I write about this question, the more I think it will need to be changed to a bug report.

## Steps to reproduce

Contents of [`ty_abs.py`](https://github.com/user-attachments/files/24600877/ty_abs.py)

```python
from typing import reveal_type

b = abs(5)
reveal_type(b)  # Runtime int
```

ty version:

```console
% ty --version
ty 0.0.11 (830cb9cc6 2026-01-09)
```

ty reveals type as Unknown:

```console
%  ty check ty_abs.py 
info[revealed-type]: Revealed type
 --> ty_abs.py:4:13
  |
3 | b = abs(5)
4 | reveal_type(b) # Runtime int
  |             ^ `Unknown`
  |

Found 1 diagnostic
```

Runtime says `int`:

```console
% python ty_abs.py
Runtime type is 'int'
```

## Other static type checkers

Mypy (1.19.1) says int:

```console
mypy --strict ty_abs.py 
ty_abs.py:4: note: Revealed type is "builtins.int"
Success: no issues found in 1 source file
```

As does pyright (1.1.408):

```console
% pyright ty_abs.py 
/Users/jeffrey/src/jpgoldberg/Python-playground/ty_abs.py
  /Users/jeffrey/src/jpgoldberg/Python-playground/ty_abs.py:4:13 - information: Type of "b" is "int"
0 errors, 0 warnings, 1 information
```

##  System information

- Python: 3.13.3 (Also tested with 3.14.0)
- ty: ty 0.0.11 (830cb9cc6 2026-01-09)
- OS: macOS 26.1
- Chip: Apple M2 Pro


### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Label `question` added by @jpgoldberg on 2026-01-13 22:19_

---

_Comment by @AlexWaygood on 2026-01-13 22:20_

This is a bug but I'm pretty confident it's #1714 

---

_Comment by @carljm on 2026-01-13 22:23_

Yes, definitely #1714 -- there's a WIP PR for that, should be fixed soon.

---

_Closed by @carljm on 2026-01-13 22:23_

---
