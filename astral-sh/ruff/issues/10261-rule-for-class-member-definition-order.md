```yaml
number: 10261
title: (🎁) Rule for class member definition order
type: issue
state: closed
author: KotlinIsland
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-03-07T00:15:02Z
updated_at: 2024-03-12T01:01:32Z
url: https://github.com/astral-sh/ruff/issues/10261
synced_at: 2026-01-12T15:54:50Z
```

# (🎁) Rule for class member definition order

---

_@KotlinIsland_

```py
class A:
    def f(self): ...  # erm, methods should come after class methods
    @classmethod
    def c(cls)   # erm, class methods should come before methods
    def __str__(self): # erm, dunders should come before methods
    s: str  # erm, attriubutes should be declared first
```
I want some kind of linting for the ordering of class members, I would like to enforce a consistent style for my codebase.

I don't know what the best practices would be, the code block is just for demonstration.

Things to consider:
- attributes
- `__new__`/`__init__`
- static methods
- class methods
- properties
- methods
- dunders

---

_Label `rule` added by @dhruvmanila on 2024-03-07 09:36_

---

_Label `needs-decision` added by @dhruvmanila on 2024-03-07 09:36_

---

_Comment by @zanieb on 2024-03-11 17:04_

Hi! Is this a duplicate of https://github.com/astral-sh/ruff/issues/2425?

Related https://github.com/astral-sh/ruff/issues/3946

---

_Comment by @KotlinIsland on 2024-03-12 00:59_

Yes, I assumed it would already be raised, so I spent ages looking in the issue tracker, couldn't find anything though. Maybe you should add something like typescripts "🔍 Search Terms" section.

- #10350

---

_Closed by @KotlinIsland on 2024-03-12 00:59_

---
