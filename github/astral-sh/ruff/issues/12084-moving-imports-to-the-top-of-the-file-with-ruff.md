---
number: 12084
title: moving imports to the top of the file with ruff
type: issue
state: closed
author: RishiMalhotra920
labels:
  - isort
assignees: []
created_at: 2024-06-28T05:52:46Z
updated_at: 2024-06-28T11:51:49Z
url: https://github.com/astral-sh/ruff/issues/12084
synced_at: 2026-01-07T13:12:15-06:00
---

# moving imports to the top of the file with ruff

---

_Issue opened by @RishiMalhotra920 on 2024-06-28 05:52_

I'm switching over from isort to ruff and one feature I dearly miss is auto-moving imports to the top of the file. I'm using ruff v0.5.0.

Say I have this file `a.py`

```
import numpy
import pandas

print("hello world")
import os

print(os, pandas, numpy)
```

Running the following commands doesn't move the `import os` to the top. 
```
ruff check --select I --fix a.py
ruff format
```

I know that with isort, running `isort .` moves the `import os` to the top.

Is there a way to enable this for standalone ruff? Also, is there a reason this isn't enabled by default?

---

_Comment by @MichaReiser on 2024-06-28 07:05_

To my knowledge, Ruff has no support for the new [`float-to-top`](https://pycqa.github.io/isort/docs/configuration/options.html#float-to-top) option yet. Did youse the new setting in your project with isort?

---

_Label `isort` added by @MichaReiser on 2024-06-28 07:05_

---

_Comment by @charliermarsh on 2024-06-28 11:51_

I think this can be merged with #1251.

---

_Closed by @charliermarsh on 2024-06-28 11:51_

---
