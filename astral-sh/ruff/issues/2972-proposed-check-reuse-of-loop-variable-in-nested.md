---
number: 2972
title: "Proposed check: reuse of loop variable in nested loop"
type: issue
state: closed
author: matthewlloyd
labels:
  - rule
assignees: []
created_at: 2023-02-16T20:49:49Z
updated_at: 2023-02-22T03:23:48Z
url: https://github.com/astral-sh/ruff/issues/2972
synced_at: 2026-01-10T01:22:41Z
---

# Proposed check: reuse of loop variable in nested loop

---

_Issue opened by @matthewlloyd on 2023-02-16 20:49_

I was just bitten by a bug in production where I accidentally reused an outer loop variable in a nested loop. Example:

```python
# Outer loop.
for i in range(10):
    # Enough code here to hide the outermost loop off screen.
    # ...

    # Developer does not notice "i" is already being used as a loop variable
    # and accidentally overwrites it here. The type is the same, so type checkers
    # don't complain.
    for i in range(10):
        print(f"inner loop {i=}")

    # Enough code here to hide the innermost loop off screen.
    # ...

    # Developer expects "i" to still hold the value of the outer loop.
    # Instead, the value is always 9.
    print(f"outer loop {i=}")
```

Proposed warning: `R???: "Variable 'i' is already declared in 'for' loop or 'with' statement above"`

Credit for the wording: this is one of PyCharm's built-in inspections.

Note that although I am personally using only Ruff, I have opened a similar enhancement request in the `flake8-bugbear` repo here: https://github.com/PyCQA/flake8-bugbear/issues/360.

---

_Label `rule` added by @charliermarsh on 2023-02-17 13:33_

---

_Comment by @matthewlloyd on 2023-02-18 02:31_

I have a working draft of this and will submit a PR in the next few days...

---

_Comment by @charliermarsh on 2023-02-18 02:33_

Sounds good. This looks reasonable to me, especially if it mirrors an existing PyCharm inspection. If bugbear adds it, we can use their rule code; otherwise, we can put it in RUF.

---

_Comment by @matthewlloyd on 2023-02-18 03:13_

Great!

We may even want to create a new category for PyCharm inspections - I looked through them all, filtered them by suitability for implementation in a linter with no type awareness, then filtered out those for which Ruff already had counterparts, and was left with no fewer than 19 total including this one:

* Assignments to 'for' loop or 'with' statement parameter (this)
* Assignment can be replaced with augmented assignment (a = a + b becomes a += b; arguably less useful)
* Class-specific decorator is used outside the class (@classmethod or @staticmethod)
* Dictionary contains duplicate keys ({'a': 1, 'a': 2})
* First argument of the method is reassigned (e.g. reassignment to self)
* Incorrect property definition (e.g. getter that does not return or yield a value)
* \_\_init\_\_ method that returns a value
* Invalid definition of 'typing.NamedTuple' (field with default followed by one or more fields without)
* Method is not declared static (but does not use self)
* Missed call to '\_\_init\_\_' of the super class
* Non-optimal list declaration (l = [1], l.append(2) becomes l = [1, 2]; arguably less useful)
* Old-style class contains new-style class features (\_\_slots\_\_, \_\_getattribute\_\_, super)
* Redeclared names without usages (def x(): pass, followed later by x = 2, no uses in between)
* Redundant boolean variable check (if b == True)
* Redundant parentheses
* Shadowing names from outer scopes
* Statement has no effect (e.g. comprises solely a variable name)
* Too complex chained comparisons ((x >= 0 and x <= 1) becomes (0 <= x <= 1)).
* Unnecessary backslash - backslashes in places where line continuation is implicit inside (), [], and {}.

---

_Comment by @matthewlloyd on 2023-02-18 03:47_

I have this tested, with snapshots, and ready to review (thanks to your excellent contribution documentation!). Just need a decision from you on whether to put this under RUF or a create new category for PyCharm (PC? PYC? CH? CHA? CHM?).

---

_Comment by @sbrugman on 2023-02-18 12:21_

Great work @matthewlloyd! `ruff` should definitely incorporate the PyCharm inspections (which I suspect are inspired by pylint).

Regarding to where they should be placed, it depends on the rule.
Some of the checks are already there (e.g. Statement has no effect => B018) or are part of pylint but not yet implemented (could be a reason to prioritize implementing them, PyCharm has autofix for many of them). Any remaining could probably be placed under `RUF`.

See also: https://github.com/PyCQA/pylint/issues/5608

Pylint

- [redefined-outer-name / W0621](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/redefined-outer-name.html#redefined-outer-name-w0621) (this)
- Assignment can be replaced with augmented assignment => __init__ method that returns a value => [return-in-init](https://vald-phoenix.github.io/pylint-errors/plerr/errors/basic/E0101.html), PLE0101)
- [consider-using-augmented-assignment](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/consider-using-augmented-assign.html) PLR6104
- [Duplicate dictionary key](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/duplicate-key.html): PLW0109 
- [bad-staticmethod-argument / W0211](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/bad-staticmethod-argument.html#bad-staticmethod-argument-w0211): Method is not declared static (but does not use self)

Let's see if we can find corresponding rules to the remaining ones in the  overview of [rules](https://pylint.readthedocs.io/en/latest/user_guide/messages/messages_overview.html) in pylint.

---

_Referenced in [astral-sh/ruff#970](../../astral-sh/ruff/issues/970.md) on 2023-02-18 12:46_

---

_Comment by @matthewlloyd on 2023-02-18 19:05_

Sounds reasonable to me! I wasn't aware of #970, and agree there is a lot of overlap. I went through all the PyCharm inspections in my list above and categorized them by whether they have PyLint counterparts, mostly by running PyLint directly on the examples provided in the PyCharm Settings panel.

The inspection in this issue is indeed partially covered by PyLint's new PLW2901 redefined-loop-name inspection, though it does not seem to be detected at all by PLW0621 redefined-outer-name. However, the semantics are slightly different between the PyCharm and PyLint versions:

* PyLint's is more general in detecting _any_ nested assignment to a loop variable, not just as the target of another loop statement.
* PyLint's is slightly more specific in working only for _loop_ variables, whereas PyCharm's also examines _with_ statements.

Given that, it might make sense to add this as PLW2901, and either roll in _with_ statements (my vote), or pull out the _with_ variant into a separate RUF. I'll need to extend my code a little to detect all nested assignments.

### Already covered by PyLint

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
* Redundant boolean variable check (if b == True)
  - PLC0121 singleton-comparison
* Statement has no effect (e.g. comprises solely a variable name)
  - PLW0104 pointless-statement
  - also B018 as above
* Too complex chained comparisons ((x >= 0 and x <= 1) becomes (0 <= x <= 1)).
  - PLR1716 chained-comparison
* Unnecessary backslash - backslashes in places where line continuation is implicit inside (), [], and {}.
  - PyLint does not detect this, but pycodestyle has E502: "The backslash is redundant between brackets" (not yet implemented in Ruff)


### PyCharm inspections not covered by PyLint in my tests

(note: this list includes only those I deemed implementable in a non-type-aware linter)

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
* Non-optimal list declaration (l = [1], l.append(2) becomes l = [1, 2]; arguably less useful)
  ```python
  l = [1]
  l.append(2)
  ```
* Redeclared names without usages (def x(): pass, followed later by x = 2, no uses in between)
  ```python
  def x(): pass
  x = 2
  ```
* Redundant parentheses
  ```python
  a = (((3)))
  ```
* Shadowing names from outer scopes
  ```python
  def outer(p):
    def inner(p):
        pass
  ```


The rule "Old-style class contains new-style class features (__slots__, __getattribute__, super)" is no longer relevant since Python 3.


---

_Referenced in [astral-sh/ruff#3022](../../astral-sh/ruff/pulls/3022.md) on 2023-02-19 01:06_

---

_Referenced in [astral-sh/ruff#3040](../../astral-sh/ruff/issues/3040.md) on 2023-02-19 19:06_

---

_Closed by @charliermarsh on 2023-02-22 03:23_

---
