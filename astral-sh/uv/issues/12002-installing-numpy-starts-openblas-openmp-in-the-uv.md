```yaml
number: 12002
title: Installing NumPy starts OpenBLAS/OpenMP in the uv process
type: issue
state: open
author: thormick
labels:
  - bug
assignees: []
created_at: 2025-03-06T13:01:28Z
updated_at: 2025-03-06T13:01:28Z
url: https://github.com/astral-sh/uv/issues/12002
synced_at: 2026-01-12T16:00:52Z
```

# Installing NumPy starts OpenBLAS/OpenMP in the uv process

---

_@thormick_

### Summary

Apparently uv starts OpenMP when it installs NumPy, causing OpenMP to spin off worker threads. When running `uv add numpy` or `uv pip install numpy` separately from `uv run python` everything seems fine, but if `uv run python` also has to install `numpy` then it starts OpenBLAS/OpenMP in the `uv` process.

```bash
$ uv --version
uv 0.6.4
$ mkdir uv_numpy_test
$ cd uv_numpy_test
$ uv init
Initialized project `uv-numpy-test`
$ uv add numpy
Using CPython 3.11.11
Creating virtual environment at: .venv
Resolved 2 packages in 93ms
Prepared 1 package in 530ms
Installed 1 package in 9ms
 + numpy==2.2.3
$ uv run python
Python 3.11.11 (main, Feb 12 2025, 14:51:05) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> ^Z
[1]+  Stopped                 uv run python
$ ps Haux | egrep "uv run python"
thormick   25095  0.0  0.1 200464 27776 pts/6    Tl   13:12   0:00 uv run python
thormick   25095  0.0  0.1 200464 27776 pts/6    Tl   13:12   0:00 uv run python
thormick   25095  0.0  0.1 200464 27776 pts/6    Tl   13:12   0:00 uv run python
thormick   25186  0.0  0.0  12160  2560 pts/6    S+   13:14   0:00 grep -E --color=auto uv run python
$ fg
uv run python

>>> ^D
$ uv pip uninstall numpy
Uninstalled 1 package in 7ms
 - numpy==2.2.3
$ uv run python
Installed 1 package in 17ms
Python 3.11.11 (main, Feb 12 2025, 14:51:05) [Clang 19.1.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> ^Z
[1]+  Stopped                 uv run python
$ ps Haux | egrep "uv run python"
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.1  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25207  0.0  0.1 776584 31744 pts/6    Tl   13:14   0:00 uv run python
thormick   25221  0.0  0.0  12160  2560 pts/6    S+   13:14   0:00 grep -E --color=auto uv run python
$
```

So, when uv installs NumPy it also loads the libraries it's shipped with, and that loads OpenBLAS, which starts OpenMP, which allocates a few megs and spawns multiprocessing worker threads (8 by default).

If I then do `import numpy` then I have two OpenMP threadpools running.

```bash
$ fg
uv run python
>>> import numpy as np
>>> ^Z
[1]+  Stopped                 uv run python
$ ps Haux | egrep "uv run python|uv_numpy_test"
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26434  0.0  0.1 776572 32000 pts/6    Tl   13:32   0:00 uv run python
thormick   26445  1.1  0.2 413704 36208 pts/6    Tl   13:32   0:00 /home/thormick/uv_numpy_test/.venv/bin/python3
thormick   26445  2.9  0.2 413704 36208 pts/6    Tl   13:32   0:00 /home/thormick/uv_numpy_test/.venv/bin/python3
thormick   26445  2.9  0.2 413704 36208 pts/6    Tl   13:32   0:00 /home/thormick/uv_numpy_test/.venv/bin/python3
thormick   26445  2.9  0.2 413704 36208 pts/6    Tl   13:32   0:00 /home/thormick/uv_numpy_test/.venv/bin/python3
thormick   26445  2.9  0.2 413704 36208 pts/6    Tl   13:32   0:00 /home/thormick/uv_numpy_test/.venv/bin/python3
thormick   26445  2.9  0.2 413704 36208 pts/6    Tl   13:32   0:00 /home/thormick/uv_numpy_test/.venv/bin/python3
thormick   26445  2.9  0.2 413704 36208 pts/6    Tl   13:32   0:00 /home/thormick/uv_numpy_test/.venv/bin/python3
thormick   26445  1.3  0.2 413704 36208 pts/6    Tl   13:32   0:00 /home/thormick/uv_numpy_test/.venv/bin/python3
thormick   26458  0.0  0.0  12172  2560 pts/6    S+   13:32   0:00 grep -E --color=auto uv run python|uv_numpy_test
$ 
```

Unlike the threads in the uv process the number of threads in the interpreter process are affected by setting the `OMP_NUM_THREADS` environment variable.

It's not really a problem for me, I was just tripped up by this when I was trying to get a handle on how NumPy and its dependencies operate, what resources they use and how to manage them. If it's just quirky behavior then it's no problem at all for me to live with, but since it might be something being run that shouldn't be or a lack of isolation somewhere then I wanted to report it.

### Platform

Ubuntu 24.04.2 LTS / Linux 6.8.0-55-generic x86_64 GNU/Linux

### Version

0.6.4

### Python version

3.11.11/3.12.3

---

_Label `bug` added by @thormick on 2025-03-06 13:01_

---
