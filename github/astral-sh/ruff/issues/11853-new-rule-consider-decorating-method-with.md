---
number: 11853
title: "New Rule: Consider decorating method with @staticmethod PYL-R0201"
type: issue
state: closed
author: Zerotask
labels: []
assignees: []
created_at: 2024-06-13T08:35:24Z
updated_at: 2024-06-13T13:04:55Z
url: https://github.com/astral-sh/ruff/issues/11853
synced_at: 2026-01-07T13:12:15-06:00
---

# New Rule: Consider decorating method with @staticmethod PYL-R0201

---

_Issue opened by @Zerotask on 2024-06-13 08:35_

Pycharm and Deepsource warns you, when you use an instance method which does not use `self` and therefore can be marked with `@staticmethod`.

> The method doesn't use its bound instance. Decorate this method with @staticmethod decorator, so that Python does not have to instantiate a bound method for every instance of this class thereby saving memory and computation. Read more about staticmethods [here](https://docs.python.org/3/library/functions.html#staticmethod).

It would be great to have that check in ruff as well.

Example:
```python
class Calculator:
    def add(self, a, b):
        return a + b
``` 

could be:
```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b
``` 

---

_Comment by @matteo-zanoni on 2024-06-13 12:46_

@Zerotask this rule has been renamed [here](https://pylint.pycqa.org/en/v3.2.3/user_guide/messages/refactor/old-no-self-use.html).
It is now **R6301** that is [already implemented](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/no_self_use.rs) in ruff.

---

_Comment by @Zerotask on 2024-06-13 13:04_

Thank you. I was blind. I actually marked the line with `# noqa: PLR6301` :D
I can confirm that it works like expected - without the noqa

---

_Closed by @Zerotask on 2024-06-13 13:04_

---
