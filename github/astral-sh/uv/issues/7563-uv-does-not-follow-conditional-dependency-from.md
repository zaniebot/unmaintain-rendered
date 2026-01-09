---
number: 7563
title: "`uv` does not follow conditional dependency from `setup.py`"
type: issue
state: closed
author: zhou13
labels:
  - duplicate
assignees: []
created_at: 2024-09-19T22:05:24Z
updated_at: 2024-09-20T00:49:17Z
url: https://github.com/astral-sh/uv/issues/7563
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv` does not follow conditional dependency from `setup.py`

---

_Issue opened by @zhou13 on 2024-09-19 22:05_

# A minimal code snippet that reproduces the bug.

```bash
cat requests-futures==0.9.8 > requirements.in
uv pip compile --python-platform x86_64-manylinux_2_31 --python-version 3.10 requirements.in --verbose
```

I got
```
DEBUG uv 0.4.12
DEBUG Starting Python discovery for Python 3.10
DEBUG Looking for exact match for request Python 3.10
DEBUG Searching for Python 3.10 in managed installations or system path
DEBUG Found `cpython-3.10.14-macos-x86_64-none` at `/Users/yichaozhou/Sources/phicore/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.10.14 interpreter at .venv/bin/python3 for builds
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.14
DEBUG Solving with target Python version: >=3.10.0
DEBUG Adding direct dependency: requests-futures==0.9.8
DEBUG Found fresh response for: https://pypi.org/simple/requests-futures/
DEBUG Searching for a compatible version of requests-futures (==0.9.8)
DEBUG Selecting: requests-futures==0.9.8 [compatible] (requests-futures-0.9.8.tar.gz)
DEBUG Acquired lock for `/Users/yichaozhou/.cache/uv/built-wheels-v3/pypi/requests-futures/0.9.8`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5c/75/bb11d835a0f1a56291f3ea87a778b41f4e5a239880feca152ad6438edcd6/requests-futures-0.9.8.tar.gz
DEBUG No static `pyproject.toml` available for: requests-futures==0.9.8 (MissingPyprojectToml)
DEBUG No static `PKG-INFO` available for: requests-futures==0.9.8 (PkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG Found static `egg-info` for: requests-futures==0.9.8
DEBUG Released lock at `/Users/yichaozhou/.cache/uv/built-wheels-v3/pypi/requests-futures/0.9.8/.lock`
DEBUG Adding transitive dependency for requests-futures==0.9.8: requests>=1.2.0
DEBUG Adding transitive dependency for requests-futures==0.9.8: futures>=2.1.3
DEBUG Found fresh response for: https://pypi.org/simple/requests/
DEBUG Found fresh response for: https://pypi.org/simple/futures/
DEBUG Searching for a compatible version of requests (>=1.2.0)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f9/9b/335f9764261e915ed497fcdeb11df5dfd6f7bf257d4a6a2a686d80da4d54/requests-2.32.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d4/ea/9d513529a89bcbcd07c8acbac9eecfad29e7562e0b9d69d14f475987ad70/futures-3.4.0-py2-none-any.whl.metadata
DEBUG Adding transitive dependency for requests==2.32.3: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.3: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
DEBUG Adding transitive dependency for requests==2.32.3: certifi>=2017.4.17
DEBUG Searching for a compatible version of futures (>=2.1.3)
DEBUG Selecting: futures==3.4.0 [compatible] (futures-3.4.0.tar.gz)
DEBUG Found fresh response for: https://pypi.org/simple/idna/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
DEBUG Found fresh response for: https://pypi.org/simple/urllib3/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ce/d9/5f4c13cecde62396b0d3fe530a50ccea91e7dfc1ccf0e09c228841bb5ba8/urllib3-2.2.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/12/90/3c9ff0512038035f59d279fddeb79f5f1eccd8859f06d6163c58798b9487/certifi-2024.8.30-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/charset-normalizer/
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.3.2 [compatible] (charset_normalizer-3.3.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/da/f1/3702ba2a7470666a62fd81c58a4c40be00670e5006a67f4d626e57f013ae/charset_normalizer-3.3.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.10 [compatible] (idna-3.10-py3-none-any.whl)
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.2.3 [compatible] (urllib3-2.2.3-py3-none-any.whl)
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2024.8.30 [compatible] (certifi-2024.8.30-py3-none-any.whl)
DEBUG Tried 7 versions: certifi 1, charset-normalizer 1, futures 1, idna 1, requests 1, requests-futures 1, urllib3 1
DEBUG Split specific environment resolution took 0.005s
Resolved 7 packages in 5ms
# This file was autogenerated by uv via the following command:
#    uv pip compile --python-platform x86_64-manylinux_2_31 --python-version 3.10 requirements.in
certifi==2024.8.30
    # via requests
charset-normalizer==3.3.2
    # via requests
     # via requests-futures
futures==3.4.0
    # via requests-futures
idna==3.10
    # via requests
requests==2.32.3
    # via requests-futures
requests-futures==0.9.8
    # via -r requirements.in
urllib3==2.2.3
    # via requests
```

However, `futures==3.4.01` should not be the dependency as according to https://github.com/ross/requests-futures/blob/v0.9.8/setup.py, `futures` should be dependency only for `python < 3.2`, while I specify `python == 3.10`.

This causes problems as `futures==3.4.01` is not installable for python 3.10:
```
$ uv pip install futures==3.4.0
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: futures==3.4.0
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
This backport is meant only for Python 2.
It does not work on Python 3, and Python 3 users do not need it as the concurrent.futures package is available in the standard library.
For projects that work on both Python 2 and 3, the dependency needs to be conditional on the Python version, like so:
extras_require={':python_version == "2.7"': ['futures']}
---
```


---

_Comment by @charliermarsh on 2024-09-19 22:12_

We do not respect package metadata that varies across distributions.

---

_Comment by @charliermarsh on 2024-09-19 22:13_

https://github.com/astral-sh/uv/issues/7170

https://github.com/astral-sh/uv/issues/7157

---

_Label `duplicate` added by @zanieb on 2024-09-19 22:25_

---

_Closed by @charliermarsh on 2024-09-20 00:49_

---
