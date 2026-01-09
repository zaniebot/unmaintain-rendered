---
number: 9842
title: False positive for B035 dictionary comprehension uses static key
type: issue
state: closed
author: rouge8
labels:
  - question
assignees: []
created_at: 2024-02-05T18:52:26Z
updated_at: 2024-02-05T19:30:33Z
url: https://github.com/astral-sh/ruff/issues/9842
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive for B035 dictionary comprehension uses static key

---

_Issue opened by @rouge8 on 2024-02-05 18:52_

## Code

```python
from dataclasses import dataclass


@dataclass
class MyObject:
    name: str


def f(obj, vals):
    return {obj.name: val for val in vals}
```

## Command

```console
❯ ruff --isolated --select B t.py
t.py:12:13: B035 Dictionary comprehension uses static key: `obj.name`
Found 1 error.
```

## Ruff version

ruff 0.2.0

---

_Comment by @charliermarsh on 2024-02-05 19:29_

Is that not a true positive? Every key is using the same value (`obj.name`), right?

---

_Label `question` added by @charliermarsh on 2024-02-05 19:29_

---

_Comment by @rouge8 on 2024-02-05 19:30_

Oh, you're right!

---

_Closed by @rouge8 on 2024-02-05 19:30_

---
