---
number: 6085
title: "`PTH201` does not detect `Path(\"\")`"
type: issue
state: closed
author: dosisod
labels: []
assignees: []
created_at: 2023-07-26T04:28:45Z
updated_at: 2023-07-26T13:22:47Z
url: https://github.com/astral-sh/ruff/issues/6085
synced_at: 2026-01-07T13:12:15-06:00
---

# `PTH201` does not detect `Path("")`

---

_Issue opened by @dosisod on 2023-07-26 04:28_

The following 3 snippets are the equivalent, though `Path("")` is not detected:

```python
from pathlib import Path

Path()     # correct code
Path("")   # no error
Path(".")  # PTH201
```

Ruff output:

```
$ ruff x.py
x.py:5:6: PTH201 [*] Do not pass the current directory explicitly to `Path`
```

Expected output:

Ruff should emit an error for `Path("")`. This is a fairly common idiom according to [grep.app](https://grep.app/search?q=%5B%5Ea-zA-Z%5DPath%5C%28%28%22%22%7C%27%27%29%5C%29&regexp=true&case=true&filter[lang][0]=Python), and is currently detected in Refurb.

---

_Comment by @harupy on 2023-07-26 12:38_

Can I fix this?

---

_Comment by @charliermarsh on 2023-07-26 12:40_

Go for it :)

---

_Referenced in [astral-sh/ruff#6095](../../astral-sh/ruff/pulls/6095.md) on 2023-07-26 13:02_

---

_Closed by @charliermarsh on 2023-07-26 13:22_

---
