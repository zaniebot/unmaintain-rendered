---
number: 17629
title: "B006: Suggest to use field() instead of None for default parameters"
type: issue
state: closed
author: C0rn3j
labels: []
assignees: []
created_at: 2025-04-25T14:40:06Z
updated_at: 2025-04-25T15:09:37Z
url: https://github.com/astral-sh/ruff/issues/17629
synced_at: 2026-01-10T01:22:59Z
---

# B006: Suggest to use field() instead of None for default parameters

---

_Issue opened by @C0rn3j on 2025-04-25 14:40_

### Summary

https://docs.astral.sh/ruff/rules/mutable-argument-default/

```python
def add_to_list(item, some_list=None):
    if some_list is None:
        some_list = []
    some_list.append(item)
    return some_list


l1 = add_to_list(0)  # [0]
l2 = add_to_list(1)  # [1]
```

The current example above makes `some_list` be able to be defined as both `None` and a `list`, which is needlessly complex and problematic as the case of None now has to be considered.

It also confuses type checkers like Pyright, which now think `add_to_list` could return both `None` and `list`.

I suggest adapting `field` from `dataclasses`, which allows to never use None, and to add typing to the suggested examples:

```python
from dataclasses import field

def add_to_list(item, some_list: list[int] = field(default_factory=list[int])) -> list[int]:
    some_list.append(item)
    return some_list

l1 = add_to_list(0)  # [0]
l2 = add_to_list(1)  # [1]
```


---

_Comment by @C0rn3j on 2025-04-25 15:09_

My bad, `field()` can only be within a `@dataclass`.

---

_Closed by @C0rn3j on 2025-04-25 15:09_

---
