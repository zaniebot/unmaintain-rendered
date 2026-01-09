---
number: 1698
title: Weird behavior on windows when creating uv venv
type: issue
state: closed
author: fervand1
labels:
  - question
assignees: []
created_at: 2024-02-19T15:17:03Z
updated_at: 2024-02-28T04:21:56Z
url: https://github.com/astral-sh/uv/issues/1698
synced_at: 2026-01-07T13:12:16-06:00
---

# Weird behavior on windows when creating uv venv

---

_Issue opened by @fervand1 on 2024-02-19 15:17_

Steps to reproduce:
- Create python 3.10 venv
- activate the venv
- pip install uv
- mkdir test
- run: uv venv
- deactivate py 3.10 venv
- activate newly created uv env  (.venv) in test
- pip list

See below
```
PS D:\Exploration\uv-pip> py -3.10 -m venv venv
PS D:\Exploration\uv-pip> .\venv\Scripts\activate
(venv) PS D:\Exploration\uv-pip> pip install uv
Looking in indexes: https://__token__:****@gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple, https://pypi.org/simple
Collecting uv
  Using cached uv-0.1.5-py3-none-win_amd64.whl (8.0 MB)
Installing collected packages: uv
Successfully installed uv-0.1.5

[notice] A new release of pip available: 22.3.1 -> 24.0
[notice] To update, run: python.exe -m pip install --upgrade pip
(venv) PS D:\Exploration\uv-pip> mkdir test


    Directory: D:\Exploration\uv-pip


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        19-02-2024     20:39                test


(venv) PS D:\Exploration\uv-pip> cd test
(venv) PS D:\Exploration\uv-pip\test> uv venv
Using Python 3.10.10 interpreter at D:\Exploration\uv-pip\venv\Scripts\python.exe
Creating virtualenv at: .venv
(venv) PS D:\Exploration\uv-pip\test> deactivate
PS D:\Exploration\uv-pip\test> ls


    Directory: D:\Exploration\uv-pip\test


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        19-02-2024     20:39                .venv


PS D:\Exploration\uv-pip\test> .\.venv\Scripts\activate
(.venv) PS D:\Exploration\uv-pip\test> pip list
DEPRECATION: Python 2.7 reached the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 is no longer maintained. pip 21.0 will drop support for Python 2.7 in January 2021. More details about Python 2 support in pip can be found at https://pip.pypa.io/en/latest/development/release-process/#python-2-support pip 21.0 will remove support for this functionality.
Package                       Version     Location
----------------------------- ----------- ----------------------------------
atomicwrites                  1.4.1
attrs                         21.4.0
backports.functools-lru-cache 1.6.6
bridgesetuptools              2.7.4
click                         7.1.2
colorama                      0.4.6
configparser                  4.0.2
contextlib2                   0.6.0.post1
decorator                     4.4.2
distlib                       0.3.6
filelock                      3.2.1
funcsigs                      1.0.2
importlib-metadata            2.1.3
importlib-resources           3.3.1
invoke                        1.7.3
jsonpath-ng                   1.6.1
jsonpath-rw                   1.4.0
lxml                          4.9.3
more-itertools                5.0.0
opcda-kepware                 1.1.0       d:\product_setup\opcda_kepware\src
packaging                     20.9
pathlib2                      2.3.7.post1
Pillow                        6.2.2
pip                           20.3.4
pip-tools                     5.5.0
platformdirs                  2.0.2
pluggy                        0.13.1
ply                           3.11
py                            1.11.0
py2exe-py2                    0.6.9
pyparsing                     2.4.7
Pyro                          3.16
pytest                        4.6.11
pywin32                       228
reportlab                     3.5.18
scandir                       1.10.0
setuptools                    44.1.1
singledispatch                3.7.0
six                           1.16.0
typing                        3.10.0.0
untangle                      1.1.1
virtualenv                    20.15.1
wcwidth                       0.2.12
wheel                         0.37.1
zipp                          1.2.0
(.venv) PS D:\Exploration\uv-pip\test>
```
When I pip list I observe that the venv created was for python 2.7, also it pulled all the installed dependency for this python into the venv when it should have been a clean venv with minimal packages

* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform - Windows 10
* The current uv version - 0.1.5


---

_Comment by @fervand1 on 2024-02-19 15:17_

Relates to https://github.com/astral-sh/uv/issues/1693

---

_Referenced in [astral-sh/uv#1693](../../astral-sh/uv/issues/1693.md) on 2024-02-19 15:19_

---

_Comment by @zanieb on 2024-02-19 15:49_

Hi! I We don't seed `pip` by default so when you run `pip list` you're not running a command in your environment. I think you're just seeing whatever global `pip` has there. You can run `uv venv --seed` to get `pip` which should resolve this.

---

_Label `question` added by @zanieb on 2024-02-19 15:49_

---

_Comment by @henryiii on 2024-02-19 16:14_

Looks like there's work on a [`uv pip list`](https://github.com/astral-sh/uv/pull/1662) starting! Until then, you can use `pip --python venv list` to look inside the venv with the external pip. (Generally, pip can also target a venv it's not installed in, just like `uv` does by default).

On Python 3.10+, you can also use the stdlib: `python -c "from importlib import metadata; print(*metadata.packages_distributions().keys())"`.

Note all these methods will report nothing if the environment is completely empty, which it is by default.


---

_Comment by @melMass on 2024-02-19 17:15_

Not entirely a solution, but `uv pip freeze` work fine

---

_Closed by @charliermarsh on 2024-02-28 04:21_

---

_Referenced in [astral-sh/uv#12265](../../astral-sh/uv/issues/12265.md) on 2025-03-18 14:43_

---
