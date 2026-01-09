---
number: 9713
title: Override hardcode package-dependency?
type: issue
state: closed
author: woutervh
labels:
  - question
assignees: []
created_at: 2024-12-08T03:34:49Z
updated_at: 2025-10-31T14:19:12Z
url: https://github.com/astral-sh/uv/issues/9713
synced_at: 2026-01-07T13:12:18-06:00
---

# Override hardcode package-dependency?

---

_Issue opened by @woutervh on 2024-12-08 03:34_

I'm trying to find a simple solution for this problem:

the package pyautogui is quite old and has  this in setup.py:
```
 'python3-Xlib;platform_system=="Linux" and python_version>="3.0"',
 'python-xlib;platform_system=="Linux" and python_version<"3.0"',
```
see https://github.com/asweigart/pyautogui/blob/master/setup.py

Many years later the python3-fork is now unmaintained and the original library now has proper python3-support

see https://stackoverflow.com/questions/74716030/what-is-the-difference-between-python-xlib-python3-xlib-pyxlib-and-xlib-in-pyt


Problem: 
we want to use **python-Xlib**, 
but **python3-Xlib** gets installed as a dependency of **pyautogui**.

I agree this problem should be fixed at package-level, but releases are glacial.

Is there currently a workaround or quick fix available,  idealy in pyproject.toml 
 that we don't loose whenever we do  "uv sync"


Both packages install an Xlib -pythonmodule,
so we want to override the Xlib-module installed by **python3-Xlib**, with the one provided by **python-Xlib**,






---

_Comment by @zanieb on 2024-12-08 03:45_

Is this the same as https://github.com/astral-sh/uv/issues/4422? (there's a solution there, I believe)

---

_Comment by @woutervh on 2024-12-08 04:02_

@zanieb  Amazing.  I never saw sys_platform == 'never'  before


Nice elegant solution with:

```
[project]
name = "demo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "pyautogui>=0.9.54",
    "python-xlib>=0.33",
]

[tool.uv]
override-dependencies = [
    "python3-xlib; sys_platform == 'never'",
]

```

---

_Closed by @woutervh on 2024-12-08 04:03_

---

_Label `question` added by @zanieb on 2024-12-08 04:35_

---

_Comment by @charliermarsh on 2025-10-31 14:19_

We just added support for excluding dependencies entirely by name: https://github.com/astral-sh/uv/pull/16528 (available in the next release)

---
