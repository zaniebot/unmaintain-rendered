---
number: 866
title: "[Bug] unused `typing` imports are not auto-fixed when pyupgrade option is selected"
type: issue
state: closed
author: ukalwa
labels:
  - bug
assignees: []
created_at: 2022-11-21T21:49:24Z
updated_at: 2022-11-22T21:38:15Z
url: https://github.com/astral-sh/ruff/issues/866
synced_at: 2026-01-07T13:12:14-06:00
---

# [Bug] unused `typing` imports are not auto-fixed when pyupgrade option is selected

---

_Issue opened by @ukalwa on 2022-11-21 21:49_

Ruff version: 0.0.132
Ruff configuration: Default
Python version: 3.10

## Simple reproduction

```py
# test.py
from typing import List
def test_sum_of_sequence(numbers: List[int]) -> int:
    return sum(numbers)
```

```toml
# pyproject.toml
[tool.ruff]
select = ["E", "F", "U"]
target-version = "py310"
```

When I ran `ruff test.py --fix`, the file changed to:

```py
from typing import List # unused import
def test_sum_of_sequence(numbers: list[int]) -> int: # Notice typing.List changed to built-in list
    return sum(numbers)
```
I would expect it to also remove unused imports `from typing import List`. As a workaround I run the command again to remove unused imports but it would be great if ruff can do it automatically.

Let me know if there is anything I could do to help.

Thank you for this amazing package. It is super fast and works great!


---

_Label `bug` added by @charliermarsh on 2022-11-21 22:35_

---

_Comment by @charliermarsh on 2022-11-21 22:37_

Thank you for the kind words, and for the detailed report!

This is sort of a "known" bug but definitely something I'd like to fix. (This would be resolved automatically if we change Ruff to iteratively fix + re-run until the code stabilizes, as suggested in #660.)


---

_Comment by @ukalwa on 2022-11-22 15:14_

Yes, that makes sense.

If no one hasn’t started working on this, I can give it a try if you can point me in the right direction.

---

_Comment by @charliermarsh on 2022-11-22 17:23_

@ukalwa - I started on this last night! Sorry to step on your toes!

---

_Comment by @ukalwa on 2022-11-22 18:39_

Cool, no worries 😀

---

_Referenced in [astral-sh/ruff#875](../../astral-sh/ruff/pulls/875.md) on 2022-11-22 21:03_

---

_Comment by @charliermarsh on 2022-11-22 21:38_

Should be fixed by https://github.com/charliermarsh/ruff/pull/875.

---

_Closed by @charliermarsh on 2022-11-22 21:38_

---
