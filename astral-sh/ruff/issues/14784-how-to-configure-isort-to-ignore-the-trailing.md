```yaml
number: 14784
title: How to configure isort to ignore the trailing comma?
type: issue
state: closed
author: ofk
labels:
  - question
  - isort
assignees: []
created_at: 2024-12-05T09:58:58Z
updated_at: 2024-12-06T02:24:26Z
url: https://github.com/astral-sh/ruff/issues/14784
synced_at: 2026-01-12T15:54:54Z
```

# How to configure isort to ignore the trailing comma?

---

_@ofk_

The formatting of from-import statements with and without parentheses is not consistent.

There is a sentence like:

```py
from package import value
```

Let's rewrite it like:

```py
from package import (
    value,
)
```

It cannot be reverted to the original format.



---

_Comment by @MichaReiser on 2024-12-05 10:06_

Keeping the import over multiple lines if there's a trailing comma after the last imported name is a convention used by Black/Ruff and isort. You can disable this behavior by [`isort.split-on-trailing-comma`](https://docs.astral.sh/ruff/settings/#lint_isort_split-on-trailing-comma) to `false`

---

_Label `question` added by @MichaReiser on 2024-12-05 10:06_

---

_Label `isort` added by @MichaReiser on 2024-12-05 10:06_

---

_Comment by @ofk on 2024-12-06 02:22_

Thanks! I got the behavior I wanted. 🎉 

---

_Closed by @ofk on 2024-12-06 02:22_

---

_Renamed from "I001: isort behavior is inconsistent." to "How to configure isort to ignore the trailing comma?" by @dhruvmanila on 2024-12-06 02:24_

---
