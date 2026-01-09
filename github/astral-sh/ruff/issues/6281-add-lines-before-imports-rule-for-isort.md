---
number: 6281
title: "Add 'lines-before-imports' rule for isort"
type: issue
state: closed
author: buriev
labels:
  - isort
  - configuration
assignees: []
created_at: 2023-08-02T17:45:43Z
updated_at: 2023-08-14T20:22:23Z
url: https://github.com/astral-sh/ruff/issues/6281
synced_at: 2026-01-07T13:12:15-06:00
---

# Add 'lines-before-imports' rule for isort

---

_Issue opened by @buriev on 2023-08-02 17:45_

Files can have a different number of blank lines between the docstring and the import. I think that this rule would have a place in the ruff

---

_Label `isort` added by @charliermarsh on 2023-08-02 21:30_

---

_Label `configuration` added by @charliermarsh on 2023-08-02 21:30_

---

_Comment by @charliermarsh on 2023-08-02 21:31_

Where do you tend to use this? What formatting are you trying to achieve?

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-02 21:31_

---

_Comment by @buriev on 2023-08-03 05:21_

Thank you for quick reply and very cool tool!

For example, in project first file 1 blank line between docstring and imports
```
"""Docstring."""

from package1 import module1
from package2 import module2
```
In second file - 2 blank lines:
```
"""Docstring."""


from package3 import module3
from package4 import module4
```

Unfortunately, this is a common problem for me, and the rule in isort itself does not work very well.
I suggest to add 'lines-before-imports' rule for [tool.ruff.isort], like rule 'lines-after-imports'. This rule expected to format all files for similar count of blank lines between docstring and imports. 

---

_Comment by @dhruvmanila on 2023-08-03 07:17_

I agree that this would be useful and would also pair well with the existing [`lines-after-imports`](https://beta.ruff.rs/docs/settings/#isort-lines-after-imports) config.

---

_Label `waiting-on-author` removed by @dhruvmanila on 2023-08-03 07:17_

---

_Comment by @buriev on 2023-08-04 18:03_

In addition, to avoid a possible bug in this rule. After docstring comments may be placed, for example:
```
"""Docstring."""
# ruff: noqa: F403

from module import *
...
```
I think in this case the newlines between the docstring and imports should be placed after comments

---

_Comment by @charliermarsh on 2023-08-14 20:22_

Realized we already have this tracked as https://github.com/astral-sh/ruff/issues/3759.

---

_Closed by @charliermarsh on 2023-08-14 20:22_

---

_Referenced in [astral-sh/ruff#3759](../../astral-sh/ruff/issues/3759.md) on 2025-02-06 15:14_

---
