```yaml
number: 3147
title: Ruff incorrectly shows UP013 violations for functional TypedDicts that cannot be converted to class syntax
type: issue
state: closed
author: sjdemartini
labels:
  - bug
assignees: []
created_at: 2023-02-22T22:45:13Z
updated_at: 2023-02-22T23:23:27Z
url: https://github.com/astral-sh/ruff/issues/3147
synced_at: 2026-01-10T11:09:46Z
```

# Ruff incorrectly shows UP013 violations for functional TypedDicts that cannot be converted to class syntax

---

_Issue opened by @sjdemartini on 2023-02-22 22:45_

Ruff currently shows a violation for the pyupgrade rule UP013 "UP013 Convert X from TypedDict functional to class syntax", even if the TypedDict `X` cannot be converted to class syntax since it has keys that aren't valid identifiers.

Example code that produces this violation:

```python
from typing import TypedDict
Point2D = TypedDict('Point2D', {'in': int, 'x-y': int})
```

Ruff version: 0.0.251

---

**More details**: 

Per the python official docs [here](https://docs.python.org/3/library/typing.html#typing.TypedDict):

> The functional syntax should also be used when any of the keys are not valid [identifiers](https://docs.python.org/3/reference/lexical_analysis.html#identifiers), for example because they are keywords or contain hyphens. Example:
>    ```
>    # raises SyntaxError
>    class Point2D(TypedDict):
>        in: int  # 'in' is a keyword
>        x-y: int  # name with hyphens
>    
>    # OK, functional syntax
>    Point2D = TypedDict('Point2D', {'in': int, 'x-y': int})
>    ```

As such, Ruff's current behavior of showing a violation of "UP013 Convert Point2D from TypedDict functional to class syntax" in the above functional syntax example seems undesirable, since it's actually the recommended/only way to use TypedDict. Rather, Ruff should just silently ignore. (pyupgrade correctly ignores this scenario.) 

Originally discussed here https://github.com/charliermarsh/ruff/issues/1212#issuecomment-1440902486

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-22 23:05_

---

_Label `bug` added by @charliermarsh on 2023-02-22 23:05_

---

_Closed by @charliermarsh on 2023-02-22 23:23_

---
