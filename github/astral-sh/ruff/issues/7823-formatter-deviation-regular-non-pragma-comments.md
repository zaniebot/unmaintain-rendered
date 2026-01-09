---
number: 7823
title: "formatter deviation: regular/non-pragma comments and line-length"
type: issue
state: closed
author: raylu
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-10-04T23:02:24Z
updated_at: 2023-10-05T16:01:42Z
url: https://github.com/astral-sh/ruff/issues/7823
synced_at: 2026-01-07T13:12:15-06:00
---

# formatter deviation: regular/non-pragma comments and line-length

---

_Issue opened by @raylu on 2023-10-04 23:02_

with the default line-length of 88, this 119-wide line
```py
a = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"  # bbbbbbbbb bbbbbbbbb bbbbbbbbb bbbbbbbbb bbbbbbbbb bbbbbbbbb
```

```
$ black --diff test.py
All done! ✨ 🍰 ✨
1 file would be left unchanged.

$ black --version
black, 23.9.1 (compiled: no)
Python (CPython) 3.9.11
```

```
$ ruff format test.py 
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
1 file reformatted

$ cat test.py
a = (
    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
)  # bbbbbbbbb bbbbbbbbb bbbbbbbbb bbbbbbbbb bbbbbbbbb bbbbbbbbb

$ ruff --version
ruff 0.0.292
```

I noticed pragma comments were a documented deviation, so I was surprised regular/non-pragma comments had this behavior. sorry if this is covered somewhere and I missed it

---

_Label `formatter` added by @charliermarsh on 2023-10-05 14:58_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-05 14:58_

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-10-05 14:58_

---

_Comment by @charliermarsh on 2023-10-05 15:03_

I think this is reasonable in that all lines now fit within the line width limit (if you increase the comment length such that the splitting doesn't help, we _don't_ split the expression). I'm gonna call this intended -- it can be closed once documented.


---

_Label `needs-decision` removed by @charliermarsh on 2023-10-05 15:03_

---

_Label `documentation` added by @charliermarsh on 2023-10-05 15:03_

---

_Comment by @charliermarsh on 2023-10-05 15:04_

(Thanks for filing, it's a great issue!)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-05 15:07_

---

_Referenced in [astral-sh/ruff#7827](../../astral-sh/ruff/pulls/7827.md) on 2023-10-05 15:52_

---

_Closed by @charliermarsh on 2023-10-05 16:01_

---
