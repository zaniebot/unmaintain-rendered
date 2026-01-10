---
number: 1408
title: "Warn if not in a virtual environment and `.venv` is present in the working directory"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - error messages
  - virtualenv
assignees: []
created_at: 2024-02-16T01:55:12Z
updated_at: 2024-02-28T08:53:18Z
url: https://github.com/astral-sh/uv/issues/1408
synced_at: 2026-01-10T01:23:06Z
---

# Warn if not in a virtual environment and `.venv` is present in the working directory

---

_Issue opened by @zanieb on 2024-02-16 01:55_

e.g. to help guard against common mistakes

---

_Label `enhancement` added by @zanieb on 2024-02-16 01:56_

---

_Label `error messages` added by @zanieb on 2024-02-16 01:56_

---

_Referenced in [astral-sh/uv#1505](../../astral-sh/uv/pulls/1505.md) on 2024-02-16 15:50_

---

_Comment by @henryiii on 2024-02-16 22:15_

FWIW, `py` ([the Python launcher for Unix](https://github.com/brettcannon/python-launcher)) automatically uses `.venv` if found without requiring it to be activated.

---

_Comment by @zanieb on 2024-02-16 22:39_

We also do the same if it's in the current directory (I'm 90% sure)

---

_Comment by @sanders41 on 2024-02-17 09:09_

> We also do the same if it's in the current directory (I'm 90% sure)

In my testing this is correct. I have not used Python launcher so I could be wrong but I think the difference will come when running the program if no virtual environment is active. `py something.py` works but `python something.py` fails even though the packages were installed into the virtual environment by `uv` since it isn’t active. 

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:38_

---

_Comment by @konstin on 2024-02-28 08:53_

We want to improve the handling of venvs, but rather by improving `uv` to make these more convenient than by showing a warning when it's possible to have workflows that don't require activating the venv (https://github.com/astral-sh/uv/pull/1505#issuecomment-1968506663)

---

_Closed by @konstin on 2024-02-28 08:53_

---
