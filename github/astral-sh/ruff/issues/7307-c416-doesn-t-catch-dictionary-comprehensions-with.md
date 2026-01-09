---
number: 7307
title: "`C416` doesn't catch dictionary comprehensions with tuple targets"
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
  - good first issue
  - accepted
assignees: []
created_at: 2023-09-12T15:23:42Z
updated_at: 2023-09-14T15:53:17Z
url: https://github.com/astral-sh/ruff/issues/7307
synced_at: 2026-01-07T13:12:15-06:00
---

# `C416` doesn't catch dictionary comprehensions with tuple targets

---

_Issue opened by @adamtheturtle on 2023-09-12 15:23_

`pylint 2.17.5` shows a R1721 error with the following file:

```python
# example.py
"""Code which has R1721 error in pylint but not ruff."""

[(k, v) for k, v in {}.items()]

```

ruff 0.0.288 shows no error for this:

```
ruff --select=ALL --isolated example.py
```

I expect `ruff` to show an error in this case.
https://github.com/astral-sh/ruff/issues/970 has `unnecessary-comprehension / R1721` checked as done.

---

_Comment by @charliermarsh on 2023-09-12 15:29_

`R1721` is implemented as a different rule: https://beta.ruff.rs/docs/rules/unnecessary-comprehension/ (C416). I just updated the issue to reflect that. However, `C416` isn't catching this case. I think it should.

---

_Renamed from "Example of R1721 being caught by pylint but not ruff" to "`C416` doesn't catch dictionary comprehensions with tuple targets" by @charliermarsh on 2023-09-12 15:29_

---

_Label `bug` added by @charliermarsh on 2023-09-12 15:29_

---

_Label `good first issue` added by @charliermarsh on 2023-09-12 15:29_

---

_Label `accepted` added by @charliermarsh on 2023-09-12 15:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-13 18:41_

---

_Referenced in [astral-sh/ruff#7363](../../astral-sh/ruff/pulls/7363.md) on 2023-09-13 19:08_

---

_Closed by @charliermarsh on 2023-09-14 15:53_

---
