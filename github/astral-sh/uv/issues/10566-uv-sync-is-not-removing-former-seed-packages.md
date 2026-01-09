---
number: 10566
title: uv sync is not removing former seed packages
type: issue
state: closed
author: adriengardou
labels:
  - bug
assignees: []
created_at: 2025-01-13T14:56:25Z
updated_at: 2025-01-13T17:43:12Z
url: https://github.com/astral-sh/uv/issues/10566
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync is not removing former seed packages

---

_Issue opened by @adriengardou on 2025-01-13 14:56_

It is stated in venv creation (https://docs.astral.sh/uv/reference/cli/#uv-venv) with `--seed` option that :

>     Install seed packages (one or more of: pip, setuptools, and wheel) into the virtual environment.
>     Note setuptools and wheel are not included in Python 3.12+ environments.

However,  `setuptools` and `wheel` are stated as seed packages in a python 3.12+ venv and re not remove when performing a `uv sync`.

On MacOS 15.2 with uv 0.5.18 (27d1bad55 2025-01-11)
venv creation : 
```
uv venv --python 3.12 --seed .venv312
Using CPython 3.12.7
Creating virtual environment with seed packages at: .venv312
 + pip==24.3.1
Activate with: source .venv312/bin/activate
```

Troublesome sync :  
```
source .venv312/bin/activate
uv pip install looseversion wheel setuptools
Using Python 3.12.7 environment at: .venv312
Resolved 3 packages in 1.25s
Installed 3 packages in 15ms
 + looseversion==1.3.0
 + setuptools==75.8.0
 + wheel==0.45.1


uv --verbose pip sync test-sync.txt   
DEBUG uv 0.5.18 (27d1bad55 2025-01-11)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.7-macos-x86_64-none` at `/Users/***/workspace/.venv312/bin/python3` (active virtual environment)
Using Python 3.12.7 environment at: .venv312
DEBUG Acquired lock for `.venv312`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: docopt-ng*
DEBUG Adding direct dependency: macholib*
DEBUG Found stale response for: https://***/docopt-ng/
DEBUG Sending revalidation request for: https://***/docopt-ng/
DEBUG Found stale response for: https://***/macholib/
DEBUG Sending revalidation request for: https://***/macholib/
DEBUG Found modified response for: https://***/docopt-ng/
WARN Skipping file for docopt-ng: docopt_ng-0.6.3-py3.6.egg
DEBUG Searching for a compatible version of docopt-ng (*)
DEBUG Selecting: docopt-ng==0.9.0 [compatible] (docopt_ng-0.9.0-py3-none-any.whl)
DEBUG Found modified response for: https://***/macholib/
WARN Skipping file for macholib: macholib-1.0-py2.3.egg
WARN Skipping file for macholib: macholib-1.0-py2.4.egg
WARN Skipping file for macholib: macholib-1.1-py2.4.egg
WARN Skipping file for macholib: macholib-1.2.dev-r25.tar.gz
WARN Skipping file for macholib: macholib-1.3-py2.5.egg
WARN Skipping file for macholib: macholib-1.3-py2.6.egg
WARN Skipping file for macholib: macholib-1.3-py2.7.egg
WARN Skipping file for macholib: macholib-1.3-py3.1.egg
WARN Skipping file for macholib: macholib-1.3-py3.2.egg
DEBUG Searching for a compatible version of macholib (*)
DEBUG Selecting: macholib==1.16.3 [compatible] (macholib-1.16.3-py2.py3-none-any.whl)
DEBUG Tried 2 versions: docopt-ng 1, macholib 1
DEBUG marker environment resolution took 0.860s
Resolved 2 packages in 861ms
DEBUG Requirement already cached: macholib==1.16.3
DEBUG Requirement already cached: docopt-ng==0.9.0
DEBUG Unnecessary package: looseversion==1.3.0
DEBUG Preserving seed package: pip==24.3.1
DEBUG Preserving seed package: setuptools==75.8.0
DEBUG Preserving seed package: wheel==0.45.1
DEBUG Uninstalled looseversion (9 files, 3 directories)
Uninstalled 1 package in 3ms
Installed 2 packages in 7ms
 + docopt-ng==0.9.0
 - looseversion==1.3.0
 + macholib==1.16.3
DEBUG Released lock at `/Users/***/workspace/.venv312/.lock`
```

Content of `test-sync.txt`
```
docopt-ng
macholib
```

---

_Label `bug` added by @charliermarsh on 2025-01-13 15:23_

---

_Comment by @charliermarsh on 2025-01-13 15:24_

Thanks, that's a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-13 17:11_

---

_Referenced in [astral-sh/uv#10572](../../astral-sh/uv/pulls/10572.md) on 2025-01-13 17:17_

---

_Closed by @charliermarsh on 2025-01-13 17:43_

---

_Closed by @charliermarsh on 2025-01-13 17:43_

---
