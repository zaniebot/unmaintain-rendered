```yaml
number: 13442
title: This confuses me
type: issue
state: closed
author: allendred
labels:
  - question
assignees: []
created_at: 2025-05-14T03:28:33Z
updated_at: 2025-05-20T14:58:21Z
url: https://github.com/astral-sh/uv/issues/13442
synced_at: 2026-01-12T16:01:29Z
```

# This confuses me

---

_@allendred_

### Question

```➜  Desktop uvx python
Python 3.12.8 (main, Dec 19 2024, 14:22:58) [Clang 18.1.8 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import pandas
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'pandas'
>>> ^D
➜  Desktop uv pip install --system pandas --break-system-packages
Using Python 3.13.3 environment at: /opt/homebrew/opt/python@3.13/Frameworks/Python.framework/Versions/3.13
Audited 1 package in 3ms
➜  Desktop```

### Platform

mac 15.5 arm

### Version

uv 0.7.3

---

_Label `question` added by @allendred on 2025-05-14 03:28_

---

_Comment by @konstin on 2025-05-14 07:26_

Please describe your problem, step-by-step what you expect and what you see instead, I can't follow along what confuses you.

---

_Comment by @ethanc-ec on 2025-05-15 12:00_

It seems that you're using two different versions of Python, the initial one is `3.12.8` while `uv` is installing Pandas to `3.13.3`. Creating separate directories for projects and using `uv init` and different `pyproject.toml` files would be beneficial.

---

_Label `needs-mre` added by @zanieb on 2025-05-15 12:31_

---

_Closed by @charliermarsh on 2025-05-20 14:17_

---

_Label `needs-mre` removed by @konstin on 2025-05-20 14:58_

---
