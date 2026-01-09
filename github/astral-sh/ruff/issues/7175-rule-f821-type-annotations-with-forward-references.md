---
number: 7175
title: Rule F821 Type Annotations With Forward References
type: issue
state: closed
author: mikedh
labels:
  - question
assignees: []
created_at: 2023-09-06T00:50:41Z
updated_at: 2025-05-13T03:32:30Z
url: https://github.com/astral-sh/ruff/issues/7175
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule F821 Type Annotations With Forward References

---

_Issue opened by @mikedh on 2023-09-06 00:50_

Thanks for the great library!! I've been loving unifying around the ruff rules. 

I was adding type hints using [PEP 484 forward references](https://peps.python.org/pep-0484/#forward-references) to avoid an import of a soft dependency. The PEP says "when a type hint contains names that have not been defined yet, that definition may be expressed as a string literal, to be resolved later." The code runs fine, and the forward reference looks OK in sphinx docs. However, they hit the `F821` rule in the same way that an unrunnably undefined type hint would. Suggested fix: make forward references with undefined name raise a different error like `F822` so it can be selectively ignored rather than ignoring all instances of `F821` which is very helpful. 

Example:
```
class Thing:
    def a_sparse(self) -> "scipy.sparse.coo_matrix":
        # this function runs fine
        # doing the import in the function to keep the dependency optional
        from scipy.sparse import coo_matrix
        return coo_matrix([0])

    def broken(self) -> ActuallyUndefined:
        # this function is actually broken by the type hint
        # and must be removed to run in Python
        return 'Nah'
```

Ruff output:
```
mikedh@orion$ ruff repro.py
repro.py:2:28: F821 Undefined name `scipy`
repro.py:7:25: F821 Undefined name `ActuallyUndefined`
Found 2 errors.
```



Version `ruff==0.0.278`
Rules:
```
[tool.ruff]
target-version = "py37"
select = [
    # "ANN", # annotations
    "B", # bugbear
    "C", # comprehensions
    "E", # style errors
    "F", # flakes
    "I", # import sorting
    "RUF100", # meta
    "U", # upgrade
    "W", # style warnings
    "YTT", # sys.version
]
ignore = [
  "C901", # Comprehension is too complex (11 > 10)
  "N802", # Function name should be lowercase
  "N806", # Variable in function should be lowercase
  "E501", # Line too long ({width} > {limit} characters)
  "B904", # raise ... from err
  "B905", # zip() without an explicit strict= parameter
  "ANN101", # type hint for `self`
  "ANN002", # type hint for *args
  "ANN003", # type hint for **kwargs
]
line-length = 90
```


---

_Renamed from "Type Annotations With Forward References" to "Rule F821 Type Annotations With Forward References" by @mikedh on 2023-09-06 00:51_

---

_Comment by @zanieb on 2023-09-06 01:05_

Hey @mikedh glad you like Ruff and thanks for the well written issue :)

A type checker will also complain that the name is undefined e.g. Pylance in VSCode reports the violation and mypy does as well:

```python
class Thing:
    def example(self) -> "bar":
        from foo import bar

        return bar()
```
```
❯ mypy example.py
example.py:2: error: Name "bar" is not defined  [name-defined]
Found 1 error in 1 file (checked 1 source file)
```

and they're correct that this name is not usable for type checking. If you move the import out of the method, e.g.

```python
class Thing:
    def example(self) -> "bar":
        return bar()
    
from foo import bar
```

there are no longer complaints of an undefined name — this is what PEP 484 generally means by deferred resolution.

I think what you're looking for is:

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from foo import bar

class Thing:
    def example(self) -> "bar":
        from foo import bar
        return bar()
```

Unfortunately this requires two imports since you need the name at runtime as well. I can see the appeal of avoiding this by introducing a separate code you can just ignore, but I'm not sure there's a point to defining the return type if it cannot be resolved by type checkers. 


---

_Label `question` added by @zanieb on 2023-09-06 01:07_

---

_Comment by @mikedh on 2023-09-07 20:37_

Thanks for the suggestions!! I just did additional imports in my case. I guess I figured since one case is runnable and one isn't it would be nice to selectively ignore it by breaking it out into a different rule. Totally understand if that's not a reasonable thing to support in ruff!

---

_Closed by @mikedh on 2023-09-07 20:37_

---

_Referenced in [astral-sh/ruff#8700](../../astral-sh/ruff/issues/8700.md) on 2023-11-15 17:55_

---

_Comment by @AntiSol on 2025-05-13 03:32_

>  I'm not sure there's a point to defining the return type if it cannot be resolved by type checkers.

..................The point would be to enable humans to know what typing is desired.

---

_Referenced in [astral-sh/ruff#10451](../../astral-sh/ruff/issues/10451.md) on 2025-12-01 17:55_

---
