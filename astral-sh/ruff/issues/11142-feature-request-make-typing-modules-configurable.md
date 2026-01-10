```yaml
number: 11142
title: "Feature request: make `typing_modules` configurable"
type: issue
state: closed
author: bersbersbers
labels:
  - question
  - configuration
assignees: []
created_at: 2024-04-25T08:07:50Z
updated_at: 2024-04-25T15:23:04Z
url: https://github.com/astral-sh/ruff/issues/11142
synced_at: 2026-01-10T11:09:53Z
```

# Feature request: make `typing_modules` configurable

---

_Issue opened by @bersbersbers on 2024-04-25 08:07_

The [`typeguard` package](https://github.com/agronholm/typeguard) enables run-time type checks. One of the drawbacks of run-time  type checks is that type annotations behind `if typing.TYPE_CHECKING` are not visible (https://typeguard.readthedocs.io/en/stable/userguide.html#notes-on-forward-reference-handling; compare also https://github.com/agronholm/typeguard/issues/456).

So this passes in `python bug.py` while it shouldn't (ideally):

`bug.py`
```python
from typing import TYPE_CHECKING

from typeguard import typechecked

if TYPE_CHECKING:
    A = int

@typechecked
def bug() -> None:
    a: A = 1.5
    print(a)

bug()
```

One workaround that I am investigating is to use a project-local `typing_` module from which I import my own `TYPE_CHECKING`, like this:

`typing_.py`
```python
from typing import TYPE_CHECKING as _TYPE_CHECKING

def enable_runtime_typing_checks() -> bool:
    return True  # more complicated, obviously

TYPE_CHECKING = _TYPE_CHECKING or enable_runtime_typing_checks()
```

`workaround.py`
```python
from typeguard import typechecked

from typing_ import TYPE_CHECKING

if TYPE_CHECKING:
    A = int

@typechecked
def bug() -> None:
    a: A = 1.5
    print(a)

bug()
```

Then, `python workaround.py` fails as expected:

```
Traceback (most recent call last):
  File "C:\Code\project\workaround.py", line 15, in <module>
    bug()
  File "C:\Code\project\workaround.py", line 11, in bug
    a: A = 1.5
  File "c:\Code\project\.venv\Lib\site-packages\typeguard\_functions.py", line 251, in check_variable_assignment
    check_type_internal(value, annotation, memo)
  File "c:\Code\project\.venv\Lib\site-packages\typeguard\_checkers.py", line 784, in check_type_internal
    raise TypeCheckError(f"is not an instance of {qualified_name(origin_type)}")
typeguard.TypeCheckError: value assigned to a (float) is not an instance of int
```

but `A` is not used at runtime. Nice!

(Obviously, my real `A` is more complicated than that, and my example above is too simplistic as it fails on letting `enable_runtime_typing_checks` return `False` - anyway, I guess this is enough to make the following main point.)

When I apply this concept more generally, though:

`code.py`
```python
from typing_ import TYPE_CHECKING

if TYPE_CHECKING:
    from pathlib import Path

    from typing_ import A

def bug() -> None:
    a: A = 1.5
    print(a)
    p: Path

bug()
```

`ruff check --isolated --select=TCH003 code.py`

gives me

```
code.py:4:25: TCH003 Move standard library import `pathlib.Path` into a type-checking block
```

So, long story short: can I make `ruff` understand my own `typing_.TYPE_CHECKING` as a type-checking block, similar to `typing.TYPE_CHECKING` and `typing_extensions.TYPE_CHECKING` (compare https://github.com/astral-sh/ruff/pull/8429)?

---

_Comment by @AlexWaygood on 2024-04-25 08:20_

Hi, thanks for the issue! We already have a `typing-modules` configuration option: https://docs.astral.sh/ruff/settings/#lint_typing-modules. Does that do what you're asking for here?

---

_Label `question` added by @AlexWaygood on 2024-04-25 08:24_

---

_Label `configuration` added by @AlexWaygood on 2024-04-25 08:24_

---

_Comment by @bersbersbers on 2024-04-25 08:29_

@AlexWaygood absolutely, thank you! I feel somewhat embarrassed since I remember asking a similar question before, but could not find it in my emails or otherwise (I now know it's https://github.com/astral-sh/ruff/issues/5400). I also searched the repository for `typing_modules` and did not find much, and I checked `ruff check . --show-settings` and could not find it there (although I clearly see it now).

I think the main reason why I went looking is because I checked https://docs.astral.sh/ruff/rules/typing-only-standard-library-import/ first, and the rule did not mention the configuration option. Maybe it should be added there (any in other `TCH` rules) to make it more discoverable.

---

_Closed by @charliermarsh on 2024-04-25 15:23_

---
