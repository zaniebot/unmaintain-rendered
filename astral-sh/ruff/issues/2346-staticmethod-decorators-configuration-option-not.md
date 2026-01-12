```yaml
number: 2346
title: "`staticmethod-decorators` configuration option not working."
type: issue
state: closed
author: ngnpope
labels:
  - question
assignees: []
created_at: 2023-01-30T10:44:25Z
updated_at: 2023-02-08T22:17:41Z
url: https://github.com/astral-sh/ruff/issues/2346
synced_at: 2026-01-12T15:54:42Z
```

# `staticmethod-decorators` configuration option not working.

---

_@ngnpope_

This is a bit of an odd one. Let's start with the ~minimal reproducer to refer to:

```python
from __future__ import annotations

import enum
import sys
import typing

if sys.version_info >= (3, 11):
    StrEnum = enum.StrEnum
else:
    # Or use backports.strenum.StrEnum
    class StrEnum(str, enum.Enum):
        pass

if typing.TYPE_CHECKING:
    from collections.abc import Callable
    from typing import Any


if typing.TYPE_CHECKING or sys.version_info >= (3, 10):
    staticmethod_hack = staticmethod
else:
    # Python < 3.10 does not support calling static methods direct from the class body.
    # This happens with the weirdness that the enum module uses when generating members.
    # As we only need to add @staticmethod for mypy, just substitute a dummy decorator.
    def staticmethod_hack(f: Callable[..., Any]) -> Callable[..., Any]:
        return f


class Animal(StrEnum):
    # Override the default as enum.StrEnum forces lowercase values.
    @staticmethod
    def _generate_next_value_(
        name: str, start: int, count: int, last_values: list[Any]
    ) -> str:
        return name.upper()

    CAT = enum.auto()
    DOG = enum.auto()
```

Python's `enum` does some weird things. Normally linters complain that [`_generate_next_value_`](https://docs.python.org/3/library/enum.html#enum.Enum._generate_next_value_), as a method, should have the first argument named `self`. There are also issues with `mypy` - see [this](https://github.com/python/mypy/issues/7591), and the solution there is to add a `@staticmethod` decorator.

However, on Python < 3.10 it isn't possible to define `_generate_next_value_()` inside the same class in which we define members. This is because until Python 3.10, static methods could not be called from within the class body, which is what happens when an enum class is created. _(So you can split into a separate `AnimalBase` and `Animal` to work around this.)_

So the idea in the above example is to define a dummy decorator for runtime on Python < 3.10. And this works:

```console
❯ mypy --version
mypy 0.991 (compiled: yes)

❯ mypy --strict bug.py 
Success: no issues found in 1 source file
```

But `N805` doesn't understand this. Looking at the configuration options, `ruff` has [`staticmethod-decorators`](https://github.com/charliermarsh/ruff#staticmethod-decorators) for `pep8-naming`. But this doesn't seem to work...

```console
❯ ruff --version
ruff 0.0.237

❯ ruff --select N805 bug.py 
bug.py:33:9: N805 First argument of a method should be named `self`
Found 1 error.

❯ cat > ruff.toml <<EOF
∙ [pep8-naming]
∙ staticmethod-decorators = ["staticmethod", "staticmethod_hack"]
∙ EOF

❯ ruff --config ruff.toml --select N805 bug.py 
bug.py:33:9: N805 First argument of a method should be named `self`
Found 1 error.

❯ cat > ruff.toml <<EOF
∙ [pep8-naming]
∙ staticmethod-decorators = ["staticmethod_hack"]
∙ EOF

❯ ruff --config ruff.toml --select N805 bug.py 
bug.py:33:9: N805 First argument of a method should be named `self`
Found 1 error.

❯ rm ruff.toml
❯ # Switch back to `@staticmethod` to show the normal decorator works...
❯ sed -i 31s/_hack// bug.py
❯ ruff --select N805 bug.py
❯ # No errors were raised...
```

So either `staticmethod-decorators` doesn't work properly, or the above way of defining the decorator is confusing `ruff`... 🤔 

---

_Comment by @charliermarsh on 2023-01-30 12:16_

I'm assuming the decorator here, in the example:

```py
class Animal(StrEnum):
    # Override the default as enum.StrEnum forces lowercase values.
    @staticmethod
    def _generate_next_value_(
        name: str, start: int, count: int, last_values: list[Any]
    ) -> str:
        return name.upper()

    CAT = enum.auto()
    DOG = enum.auto()
```

Should be `@staticmethod_hack`?

Right now, Ruff is gonna have trouble tracking that it's `@staticmethod_hack` is sometimes equal to `@staticmethod`, unfortunately.

I suppose one workaround would be to move that `@staticmethod_hack` into a separate file, then import it, and add that path to `staticmethod-decorators`, like `staticmethod-decorators = ["my_file.staticmethod_hack"]`. I haven't tried it... but I assume that would work.


---

_Label `question` added by @charliermarsh on 2023-01-30 12:16_

---

_Comment by @ngnpope on 2023-01-30 12:30_

> Should be @staticmethod_hack?

Yes 🤦🏻 Copy-pasted the wrong version.

> Ruff is gonna have trouble tracking that it's `@staticmethod_hack` is sometimes equal to `@staticmethod`, unfortunately.

Ok, I guess this is one of those too-dynamic things... It's easy enough to use `# noqa: N805` or split out the method to a separate enum base until 3.10 becomes the minimum version. I'd just wondered if there was something that was overlooked.

Feel free to close this one out if there's nothing to action. It can serve as documentation for someone searching the issue tracker.

---

_Comment by @charliermarsh on 2023-01-30 12:33_

I appreciate it, thanks for all the info!

---

_Closed by @charliermarsh on 2023-01-30 12:33_

---

_Comment by @l0b0 on 2023-02-08 22:17_

Thank you for `staticmethod_hack`! Unfortunately, `pylint` has some issues with it:

> Class name "staticmethod_hack" doesn't conform to PascalCase naming style (invalid-name)

and

> Method should have "self" as first argument (no-self-argument)

---
