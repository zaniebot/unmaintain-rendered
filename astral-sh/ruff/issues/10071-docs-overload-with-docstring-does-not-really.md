```yaml
number: 10071
title: "docs: overload-with-docstring does not really explain \"why this is bad\""
type: issue
state: open
author: pawamoy
labels:
  - rule
assignees: []
created_at: 2024-02-21T13:50:15Z
updated_at: 2024-02-22T13:48:30Z
url: https://github.com/astral-sh/ruff/issues/10071
synced_at: 2026-01-12T15:54:49Z
```

# docs: overload-with-docstring does not really explain "why this is bad"

---

_@pawamoy_

https://docs.astral.sh/ruff/rules/overload-with-docstring/

Current explanation:

> The `@overload` decorator is used to define multiple compatible signatures for a given function, to support type-checking. A series of `@overload` definitions should be followed by a single non-decorated definition that contains the implementation of the function.

> `@overload` function definitions should not contain a docstring; instead, the docstring should be placed on the non-decorated definition that contains the implementation.

The references at the end say nothing about docstrings in overloads:

- [PEP 257 – Docstring Conventions](https://peps.python.org/pep-0257/)
- [Python documentation: typing.overload](https://docs.python.org/3/library/typing.html#typing.overload)

---

To me, docstrings in overloads *can be* a bad thing, because these docstrings are lost at runtime. Therefore they cannot be displayed with `help(function)` in a REPL or in other interactive interpreters (notebooks).

However, in a static context (IDE, documentation tools), they *could* be a good thing: currently VSCode shows a slider with each overloaded docstring when displaying help for parameters while typing:

![vscode_overloads](https://github.com/astral-sh/ruff/assets/3999221/99d9ca00-e4cf-4dda-8859-e1530f581429)
![vscode_overloads2](https://github.com/astral-sh/ruff/assets/3999221/7381130b-32ff-460a-9a99-2c592ace3b48)

When the docstring is written in the implementation, VSCode still shows a slider. The only thing that changes then between the popups is the function signature.

<details><summary>The code if you want to test:</summary>

```python
from typing import overload


@overload
def f(a: int, b: str) -> str:
    ...


@overload
def f(c: int, d: str) -> str:
    ...


def f(
    a: int | None = None,
    b: str | None = None,
    c: int | None = None,
    d: str | None = None,
) -> str:
    """F.
    
    Args:
        a: Third.
        b: Third.
        c: Third.
        d: Third.
    """
    return b or d


f()
```

</details>

Any other suggestions *why* docstrings in overloads are a good or a bad thing?

---

_Comment by @KotlinIsland on 2024-02-22 02:42_

They aren't lost at runtime.
```py
from typing import overload, get_overloads
@overload
def f():
    """f1"""
@overload
def f():
    """f2"""
def f():
    """f3"""

for fn in get_overloads(f):
    help(fn)
```

---

_Comment by @pawamoy on 2024-02-22 09:24_

Ah, interesting, thanks! The overloads are a registered in a global registry. Then I don't have any argument for why adding docstrings to overloads is a bad thing :smile:

---

_Label `rule` added by @MichaReiser on 2024-02-22 13:48_

---
