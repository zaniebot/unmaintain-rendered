```yaml
number: 3538
title: D400 auto-fix is incorrect
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-03-15T10:48:32Z
updated_at: 2023-03-17T02:36:27Z
url: https://github.com/astral-sh/ruff/issues/3538
synced_at: 2026-01-12T15:54:43Z
```

# D400 auto-fix is incorrect

---

_@JonathanPlasse_

I use PEP 257 convention and `select=ALL`.
```python
def check_clockwise(polygon: List[LatLong]) -> bool:
    """Checks if a polygon is declared (mostly) clockwise,
    or if it's in the correct orientation"""
```
becomes
```python
def check_clockwise(polygon: List[LatLong]) -> bool:
    """Checks if a polygon is declared (mostly) clockwise,
    or if it's in the correct orientation
    .
    """
```
D400 is declared as always auto-fixable.
I would not add a period if there is no empty line between the first line and the rest of the docstring like in the case above.

---

_Comment by @charliermarsh on 2023-03-15 19:53_

It puts the period on its own line?

---

_Comment by @JonathanPlasse on 2023-03-15 20:08_

Yes

---

_Comment by @charliermarsh on 2023-03-15 23:35_

Very sad!

---

_Label `bug` added by @charliermarsh on 2023-03-15 23:36_

---

_Label `autofix` added by @charliermarsh on 2023-03-15 23:36_

---

_Comment by @charliermarsh on 2023-03-15 23:39_

Ah I think this is `D209` and `D400` competing, the fixes conflict, we end up adding the newline _after_ the period. If I _just_ fix `D400`, it works as expected:

```py
def check_clockwise(polygon: List[LatLong]) -> bool:
    """Checks if a polygon is declared (mostly) clockwise,
    or if it's in the correct orientation."""
```

I think the expected outcome, as long as you have  here is:

```py
def check_clockwise(polygon: List[LatLong]) -> bool:
    """Checks if a polygon is declared (mostly) clockwise,
    or if it's in the correct orientation.
    """
```

Or even:

```py
```py
def check_clockwise(polygon: List[LatLong]) -> bool:
    """Checks if a polygon is declared (mostly) clockwise,
    or if it's in the correct orientation."""
```

If we're considering the "summary line" to be the first logical line, rather than the first physical line.


---

_Closed by @charliermarsh on 2023-03-17 02:36_

---
