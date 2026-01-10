```yaml
number: 11763
title: "[New rules request] Empty class and class with two methods, one of which is init"
type: issue
state: closed
author: Rizhiy
labels: []
assignees: []
created_at: 2024-06-05T22:06:02Z
updated_at: 2024-09-09T22:53:21Z
url: https://github.com/astral-sh/ruff/issues/11763
synced_at: 2026-01-10T11:09:53Z
```

# [New rules request] Empty class and class with two methods, one of which is init

---

_Issue opened by @Rizhiy on 2024-06-05 22:06_

Hi, not sure if this is the correct place, please point me in the right direction if not.
I've recently re-watched a [good talk on not writing too many classes](https://youtube.com/watch?v=o9pEzgHorH0) and was wondering if it is possible to implement a few recommendations giver there as rules.

__Empty class rule__: warn when the class just subclasses another while not doing anything, e.g.:
```python
class MyDict(dict):
	pass
```

__Class with two methods, one of which is `__init__`__: warn when a class has only two methods, one of which is `__init__`, e.g.:
```python
class Greeting:
    def __init__(self, greeting: str = "Hello") -> None:
        self._greeting = greeting

    def greet(self, name) -> None:
        print(f"{self._greeting} {name}!")
```

I did a quick search through existing rules, but couldn't find anything relevant.


---

_Renamed from "[New rules request] Empty class and Class with two methods one of which is init" to "[New rules request] Empty class and class with two methods, one of which is init" by @Rizhiy on 2024-06-05 22:06_

---

_Comment by @charliermarsh on 2024-06-06 04:03_

I think this is roughly what's implemented in https://github.com/astral-sh/ruff/pull/9601. I might suggest chiming in on or watching that PR!

---

_Closed by @charliermarsh on 2024-06-06 04:03_

---
