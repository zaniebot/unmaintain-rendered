---
number: 6539
title: distutils is no longer in stdlib since Python 3.12 (PEP 632)
type: issue
state: closed
author: dlech
labels:
  - needs-info
assignees: []
created_at: 2023-08-13T19:36:36Z
updated_at: 2023-08-13T20:00:14Z
url: https://github.com/astral-sh/ruff/issues/6539
synced_at: 2026-01-07T13:12:15-06:00
---

# distutils is no longer in stdlib since Python 3.12 (PEP 632)

---

_Issue opened by @dlech on 2023-08-13 19:36_

Currently, when ruff sorts imports, `distutils` is grouped with other standard library packages. However, it has long been deprecated and has been removed in Python 3.12. https://peps.python.org/pep-0632/

It should now be sorted with 3rd-party packages.

Current behavior:

```python
import os
from distutils.command.install_headers import install_headers as _install_headers

from setuptools import setup
```

Expected behavior:

```python
import os

from distutils.command.install_headers import install_headers as _install_headers
from setuptools import setup
```

---

_Comment by @charliermarsh on 2023-08-13 19:51_

I believe we've already implemented this -- we treat `distutils` as third-party if your minimum Python version is set to 3.12. Otherwise, it's treated as standard library, which seems correct to me.

E.g., given the above input, if you run `ruff check foo.py --isolated --select I --fix --target-version py312`, you get:

```python
import os

from distutils.command.install_headers import install_headers as _install_headers
from setuptools import setup
```

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-13 19:51_

---

_Comment by @dlech on 2023-08-13 20:00_

Thanks. It probably should have occurred to me to try changing the target version first. 😄 

---

_Closed by @dlech on 2023-08-13 20:00_

---
