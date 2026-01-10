```yaml
number: 1090
title: ty allows passing different types for constrained TypeVar arguments.
type: issue
state: open
author: IDrokin117
labels:
  - bug
  - generics
assignees: []
created_at: 2025-08-23T15:58:43Z
updated_at: 2025-12-05T00:40:00Z
url: https://github.com/astral-sh/ty/issues/1090
synced_at: 2026-01-10T01:56:40Z
```

# ty allows passing different types for constrained TypeVar arguments.

---

_Issue opened by @IDrokin117 on 2025-08-23 15:58_

# Summary
Ty doesn't flag issues when two distinct types are passed to a function that uses the same generic type parameter T for its arguments. This might be allowed for `bound` types (if satisfy), but not for `constraints`. See examples for mypy and pyrefly.
```python
from typing import TypeVar

T = TypeVar("T", int, str) 
def q(s: T, s1: T): # s and s1 must have the same type
    pass
q(1, "str") # not OK
```
```python
from typing import TypeVar

T = TypeVar("T",  bound=int | str) 
def q(s: T, s1: T): # s and s1 might be only subtype of "int | str"
    pass
q(1, "str")  # OK
```
See [Bound versus Constraints](https://www.gaohongnan.com/computer_science/type_theory/05-typevar-bound-constraints.html#id20)
## Playgrounds
- [Ty](https://play.ty.dev/85f7a6c3-e88b-49fd-aa49-d4b85bd0ffe9). No errors
- [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=552421be9605bfe11da9e665ae663fef). ```main.py:7: error: Value of type variable "T" of "q" cannot be "object"  [type-var]```
- [Pyrefly](https://pyrefly.org/sandbox/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFCiSyKoZY55QAqihAamRD163KAF4efQSAAUAIm7yANJhQxVAZxggAlPQAmhYFACOszQC4eWgIzXuuy-SiuoCMps31zt1fO0QeV0gA&version=3.12) ```ERROR 7:6-11: Argument `Literal['str']` is not assignable to parameter `s1` with type `int` in function `q` [[bad-argument-type](https://pyrefly.org/en/docs/error-kinds/#bad-argument-type)]```

### Version

0.0.1-alpha.19

---

_Label `bug` added by @AlexWaygood on 2025-08-23 16:25_

---

_Label `generics` added by @AlexWaygood on 2025-08-23 16:25_

---

_Added to milestone `Beta` by @carljm on 2025-08-25 16:05_

---

_Comment by @carljm on 2025-08-25 16:05_

Thanks for the report! I think at this point this will likely be rolled into #623, but I'll keep it open to make sure it gets fixed.

---

_Renamed from "Ty allows passing different types for TypeVar arguments." to "Ty allows passing different types for constrained TypeVar arguments." by @AlexWaygood on 2025-08-27 11:28_

---

_Renamed from "Ty allows passing different types for constrained TypeVar arguments." to "ty allows passing different types for constrained TypeVar arguments." by @MichaReiser on 2025-09-19 08:28_

---

_Assigned to @dcreager by @carljm on 2025-09-19 14:51_

---

_Removed from milestone `Beta` by @carljm on 2025-12-05 00:40_

---

_Added to milestone `Stable` by @carljm on 2025-12-05 00:40_

---
