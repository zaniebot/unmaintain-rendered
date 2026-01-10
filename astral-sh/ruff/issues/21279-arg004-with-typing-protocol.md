---
number: 21279
title: ARG004 with typing.Protocol
type: issue
state: closed
author: Lexxx42
labels:
  - question
assignees: []
created_at: 2025-11-05T06:57:17Z
updated_at: 2025-11-06T19:16:11Z
url: https://github.com/astral-sh/ruff/issues/21279
synced_at: 2026-01-10T01:23:02Z
---

# ARG004 with typing.Protocol

---

_Issue opened by @Lexxx42 on 2025-11-05 06:57_

### Question

[unused-static-method-argument (ARG004)](https://docs.astral.sh/ruff/rules/unused-static-method-argument/#unused-static-method-argument-arg004)

how to pass a ruff check when using typing.Protocol?
```python
from dataclasses import dataclass
from typing import Protocol


class Proto(Protocol):
    """doc."""
    def __call__(self, items: list[str], name: str) -> bool:
        """Doc."""
@dataclass
class Config:
    """doc."""
    strategy: Proto
    msg: str
class Checker:
    """doc."""
    def __init__(self):
        """Doc."""
        self._strategies: dict[str, Config] = {
            "1": Config(strategy=self._is_cool, msg="1"),
            "2": Config(strategy=self._is_good, msg="2"),
            "3": Config(strategy=self._is_bad, msg="3"),
        }

    @staticmethod
    def _is_cool(items: list[str], name: str) -> bool:
        return True
    @staticmethod
    def _is_good(items: list[str], name: str) -> bool:
        return name in items

    @staticmethod
    def _is_bad(items: list[str], name: str) -> bool:
        return len(items) > 0 and name == "asd"
```

```
   |
27 |     @staticmethod
28 |     def _is_cool(items: list[str], name: str) -> bool:
   |                  ^^^^^ ARG004
29 |         return True
   |
```

```
   |
27 |     @staticmethod
28 |     def _is_cool(items: list[str], name: str) -> bool:
   |                                    ^^^^ ARG004
29 |         return True
   |
```

1. ` def _is_cool(_items: list[str], _name: str) -> bool:`

but it's ty error, which is correct:
```
error[invalid-argument-type]: Argument is incorrect
   |
24 |         """Doc."""
25 |         self._strategies: dict[str, Config] = {
26 |             "1": Config(strategy=self._is_cool, msg="1"),
   |                         ^^^^^^^^^^^^^^^^^^^^^^ Expected `Proto`, found `def _is_cool(_items: list[str], _name: str) -> bool`
27 |             "2": Config(strategy=self._is_good, msg="2"),
28 |             "3": Config(strategy=self._is_bad, msg="3"),
   |
info: rule `invalid-argument-type` is enabled by default
```

2. 
```python
    @staticmethod
    def _is_cool(items: list[str], name: str) -> bool:
        _ = items, name
        return True
```

3. noqa
`    def _is_cool(items: list[str], name: str) -> bool:  # noqa: ARG004`

Maybe there is a way to pass a check more elegantly?

### Version

ruff 0.14.3

---

_Label `question` added by @Lexxx42 on 2025-11-05 06:57_

---

_Comment by @Daverball on 2025-11-05 21:55_

Usually with callback protocols it's a good idea to either make all of the arguments positional only or keyword only, this puts fewer constraints on the implementations.

In this case if you change the protocol to:

```python
from typing import Protocol

class Proto(Protocol):
    def __call__(self, items: list[str], name: str, /) -> bool:
```

You can change your implementation to:

```python
    @staticmethod
    def _is_cool(_items: list[str], _name: str) -> bool:
```

With the preceding underscore you're signaling that you're not using those arguments, so it will no longer trigger the `ARG004` violation.

But sometimes you're of course forced to use a certain argument name and in that case you will just have to live with the `noqa` comment, it's the only solution, besides disabling the rule altogether, which is a perfectly valid decision as well.

---

_Closed by @Lexxx42 on 2025-11-06 19:16_

---
