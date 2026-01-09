---
number: 9305
title: F811 not working in specific case
type: issue
state: closed
author: albertopoljak
labels:
  - question
assignees: []
created_at: 2023-12-29T08:41:43Z
updated_at: 2023-12-31T12:40:01Z
url: https://github.com/astral-sh/ruff/issues/9305
synced_at: 2026-01-07T13:12:15-06:00
---

# F811 not working in specific case

---

_Issue opened by @albertopoljak on 2023-12-29 08:41_

RUFF rule F811 does not catch this case:

```py
from test import Test


class Test(Test):
    ...
```

When tested with PyLint:
`test2.py:4:0: E0102: class already defined line 1 (function-redefined)`

Flake8 does not flag this.

---

_Comment by @zanieb on 2023-12-29 16:09_

Hm `F811` is [`redefined-of-unused`](https://docs.astral.sh/ruff/rules/redefined-while-unused/) and your variable `Test` is being used before it is redefined, I believe this is as-designed.

---

_Label `question` added by @zanieb on 2023-12-29 16:10_

---

_Comment by @albertopoljak on 2023-12-29 20:36_

> Hm `F811` is [`redefined-of-unused`](https://docs.astral.sh/ruff/rules/redefined-while-unused/) and your variable `Test` is being used before it is redefined, I believe this is as-designed.

I found F811 to be the most similar to PyLint E0102. I also tried Ruff with ALL but to no avail.

Is there any other rule that should catch this case?

---

_Comment by @zanieb on 2023-12-29 20:45_

I don't think this is implemented another way. Maybe we need to actually implement the Pylint rule, as I think F811 is doing what it is supposed to here but it does seem useful to raise a violation for a redefined name here.

---

_Comment by @charliermarsh on 2023-12-31 12:40_

👍 I've unchecked E0102 from https://github.com/astral-sh/ruff/issues/970. Merging into that issue.

---

_Closed by @charliermarsh on 2023-12-31 12:40_

---
