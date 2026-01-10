---
number: 3164
title: Uv does not list all installed packages.
type: issue
state: closed
author: mathias-ted
labels:
  - question
assignees: []
created_at: 2024-04-20T22:00:39Z
updated_at: 2025-03-31T13:35:54Z
url: https://github.com/astral-sh/uv/issues/3164
synced_at: 2026-01-10T01:23:25Z
---

# Uv does not list all installed packages.

---

_Issue opened by @mathias-ted on 2024-04-20 22:00_


Command: uv pip list

Version: 0.1.35 

Platform: windows 10


when I run the command `pip list`, all the installed package are listed , but when i run` uv pip list,` some packages are missing See the screenshot below.


![Screenshot 2024-04-21 003809](https://github.com/astral-sh/uv/assets/105419609/0d8ffb1a-4ba8-425d-ab03-45cbada5c890)






---

_Comment by @charliermarsh on 2024-04-20 22:16_

`uv pip list` will list the packages that are installed in the current virtual environment. Where are those packages (like parso) installed?

---

_Comment by @mathias-ted on 2024-04-20 22:49_

The packages are installed in C:\Users\%usename%\AppData\Roaming\Python\Python312\site-packages.


---

_Comment by @zeynalhajili on 2024-04-21 21:58_

How can we specify where uv should install packages? in pip we do have -t flag, but here I cannot find 

---

_Comment by @zanieb on 2024-04-22 01:53_

Please see #2752 for details on `--target`. uv supports installing into the active virtual environment.

---

_Comment by @charliermarsh on 2024-04-22 14:49_

You can run with `--system` to show the packages installed in the system Python, or `--python` (with a path to a Python interpreter) to list packages for a specific Python.

---

_Closed by @charliermarsh on 2024-04-22 14:49_

---

_Label `question` added by @zanieb on 2024-04-22 14:51_

---

_Referenced in [astral-sh/uv#3341](../../astral-sh/uv/issues/3341.md) on 2024-05-03 03:40_

---

_Referenced in [TechnologyBrewery/habushu#221](../../TechnologyBrewery/habushu/pulls/221.md) on 2025-02-13 22:10_

---

_Referenced in [tensorflow/text#1393](../../tensorflow/text/issues/1393.md) on 2025-03-31 12:16_

---
