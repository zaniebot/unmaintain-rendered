---
number: 10166
title: virtualenv broken during import
type: issue
state: closed
author: nadecancode
labels: []
assignees: []
created_at: 2024-12-26T07:03:15Z
updated_at: 2024-12-26T08:01:11Z
url: https://github.com/astral-sh/uv/issues/10166
synced_at: 2026-01-10T01:24:50Z
---

# virtualenv broken during import

---

_Issue opened by @nadecancode on 2024-12-26 07:03_

Hello, I am running into issues with virtualenv.

I am trying to add `numpy` to the project and it turns out instead of the numpy added to the virtualenv folder `.venv/lib/python3.10/site-packages/numpy` it is referencing the numpy installed in the system.

```
Original error was: No module named 'numpy._core._multiarray_umath'


The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/usr/lib/python3.13/site-packages/numpy/__init__.py", line 119, in <module>
    raise ImportError(msg) from e
ImportError: Error importing numpy: you should not try to import numpy from
        its source directory; please exit the numpy source tree, and relaunch
        your python interpreter from there.
```

The virtualenv should be activated as I typed the `source .venv/bin/activate` and 
```
which python
```
outputs
```
(project prefix)/.venv/bin/python
```

and

```echo $VIRTUAL_ENV```
outputs
```
(project prefix)/.venv
```

---

_Comment by @nadecancode on 2024-12-26 08:01_

Related to dotfile configuration, https://github.com/mylinuxforwork/dotfiles/issues/566 - not UV related. Closing this.

---

_Closed by @nadecancode on 2024-12-26 08:01_

---
