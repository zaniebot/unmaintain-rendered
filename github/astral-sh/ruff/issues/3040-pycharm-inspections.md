---
number: 3040
title: PyCharm inspections
type: issue
state: open
author: matthewlloyd
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-02-19T19:06:18Z
updated_at: 2025-02-23T13:56:46Z
url: https://github.com/astral-sh/ruff/issues/3040
synced_at: 2026-01-07T13:12:14-06:00
---

# PyCharm inspections

---

_Issue opened by @matthewlloyd on 2023-02-19 19:06_

(Opening a new issue to continue the discussion from #2972 which will be closed by https://github.com/charliermarsh/ruff/pull/3022)

I went through all the PyCharm inspections, filtered those I deemed implementable in a non-type-aware linter, and categorized them by whether they have Ruff or PyLint counterparts, mostly by running Ruff and PyLint directly on the examples provided in the PyCharm Settings panel.

There were five left over, which might merit consideration for inclusion as RUF rules.

### PyCharm inspections not currently covered by Ruff or PyLint

* Class-specific decorator is used outside the class (@classmethod or @staticmethod)
  ```python
  @staticmethod
  def a(): pass
  ```
* Incorrect property definition (e.g. getter that does not return or yield a value)
  ```python
  class A:
      @property
      def field(): pass
  ```
* Invalid definition of 'typing.NamedTuple' (field with default followed by one or more fields without)
  ```python
  import typing

  class FullName(typing.NamedTuple):
      first: str
      last: str = ""
      middle: str
  ```
* Non-optimal list declaration
  ```python
  l = [1]
  l.append(2)
  ```
* Shadowing names from outer scopes
  ```python
  def outer(p):
    def inner(p):
        pass
  ```

### Covered by Ruff or PyLint

* Assignments to 'for' loop or 'with' statement parameter (see above)
  - PLW2901 redefined-loop-name
* Assignment can be replaced with augmented assignment (a = a + b becomes a += b; arguably less useful)
  - PLR6104 consider-using-augmented-assign 
* Dictionary contains duplicate keys ({'a': 1, 'a': 2})
  - PLW0109 duplicate-key
* First argument of the method is reassigned (e.g. reassignment to self)
  - PLW0642 self-cls-assignment
* __init__ method that returns a value
  - PLE101 return-in-init
* Method is not declared static (but does not use self)
  - PLR6301 no-self-use
  - PLW0211 bad-staticmethod-argument (not quite the same thing)
* Missed call to '__init__' of the super class
  - PLW0231 super-init-not-called
* Redeclared names without usages (def x(): pass, followed later by x = 2, no uses in between)
  - F811
* Redundant boolean variable check (if b == True)
  - PLC0121 singleton-comparison
* Redundant parentheses
  - UP034
* Statement has no effect (e.g. comprises solely a variable name)
  - PLW0104 pointless-statement
  - also B018 as above
* Too complex chained comparisons ((x >= 0 and x <= 1) becomes (0 <= x <= 1)).
  - PLR1716 chained-comparison
* Unnecessary backslash - backslashes in places where line continuation is implicit inside (), [], and {}.
  - PyLint does not detect this, but pycodestyle has E502: "The backslash is redundant between brackets" (not yet implemented in Ruff)


---

_Referenced in [astral-sh/ruff#970](../../astral-sh/ruff/issues/970.md) on 2023-02-19 19:08_

---

_Referenced in [astral-sh/ruff#3022](../../astral-sh/ruff/pulls/3022.md) on 2023-02-19 19:09_

---

_Comment by @charliermarsh on 2023-02-19 19:28_

Thank you for this, super thorough and much appreciated!

---

_Label `plugin` added by @charliermarsh on 2023-02-19 19:28_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @pkulev on 2024-01-17 15:14_

I'm looking forward to see those checks in ruff!
I'll expand here some moments regarding F811 and similar check in pycharm.

In the following example all redeclarations were highlighted by pycharm, but ruff alerts only on imports and function/class definitions. Variable redeclaration without usage passes the check.

```python
import os
import sys
import os  # ruff alerts F811

def foo():
    pass

def foo():  # ruff alerts F811
    pass

class Foo:
    pass

class Foo:  # ruff alerts F811
    pass

class Bar:
    x = 10
    x = 20  # No alert

BAR = "bar"
BAR = "baz"  # No alert
```

It would be great to enable checking for variables too.

---

_Referenced in [astral-sh/ruff#6903](../../astral-sh/ruff/issues/6903.md) on 2024-11-18 22:33_

---

_Referenced in [astral-sh/ruff#15903](../../astral-sh/ruff/pulls/15903.md) on 2025-02-03 06:01_

---

_Comment by @dylwil3 on 2025-02-23 13:56_

Copied from #16324 :

PyCharm catches this:

```py
class Foo:
    def define_attr(self):
        self.x = 5
```

by complaining that:

> Instance attribute x defined outside \_\_init\_\_ 

Would be a nice Ruff rule!

---

_Referenced in [astral-sh/ruff#16324](../../astral-sh/ruff/issues/16324.md) on 2025-02-23 13:57_

---
