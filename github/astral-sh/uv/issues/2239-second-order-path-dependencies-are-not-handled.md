---
number: 2239
title: Second order path dependencies are not handled
type: issue
state: closed
author: adrienball
labels: []
assignees: []
created_at: 2024-03-06T12:15:49Z
updated_at: 2024-03-06T14:30:06Z
url: https://github.com/astral-sh/uv/issues/2239
synced_at: 2026-01-07T13:12:17-06:00
---

# Second order path dependencies are not handled

---

_Issue opened by @adrienball on 2024-03-06 12:15_

I am facing an issue with second order path dependencies, which I manage to isolate in a simple example.
Let's consider the following project layout:
```console
├── package-1
│   └── setup.py
├── package-2
│   └── setup.py
└── package-3
    └── setup.py
```

With the following content:

**package-1/setup.py**
```
from setuptools import setup

setup(
    name="package-1",
    version="0.1.0"
)
```

**package-2/setup.py**
```
from setuptools import setup, os

setup(
    name="package-2",
    version="0.1.0",
    install_requires=[f"package-1 @ file://{os.getcwd()}/../package-1"],
)
```

**package-3/setup.py**
```
from setuptools import setup, os

setup(
    name="package-3",
    version="0.1.0",
    install_requires=[f"package-2 @ file://{os.getcwd()}/../package-2"],
)
```

Now if I try to install package-3:

```shell
> cd package-3
> uv venv --seed
Using Python 3.9.9 interpreter at: /Library/Frameworks/Python.framework/Versions/3.9/bin/python3
Creating virtualenv at: .venv
 + pip==24.0
 + setuptools==69.1.1
 + wheel==0.42.0
Activate with: source .venv/bin/activate
> uv pip install -e .
   Built file:///Users/###/uv-debug/package-3                                                                                                                                                                                                    Built 1 editable in 1.07s
error: Package `package-1` attempted to resolve via URL: file:///Users/###/uv-debug/package-1. URL dependencies must be expressed as direct requirements or constraints. Consider adding `package-1 @ file:///Users/###/uv-debug/package-1` to your dependencies or constraints file.
```

However if I try to install package-2, it works:
```shell
> cd package-2
> uv venv --seed
Using Python 3.9.9 interpreter at: /Library/Frameworks/Python.framework/Versions/3.9/bin/python3
Creating virtualenv at: .venv
 + pip==24.0
 + setuptools==69.1.1
 + wheel==0.42.0
Activate with: source .venv/bin/activate
> uv pip install -e .
   Built file:///Users/###/uv-debug/package-2                                                                                                                                                                                                    Built 1 editable in 1.10s
Resolved 2 packages in 1ms
Installed 2 packages in 5ms
 + package-1==0.1.0 (from file:///Users/###/uv-debug/package-1)
 + package-2==0.1.0 (from file:///Users/###/uv-debug/package-2)
```

I have no issue if I use `pip install -e .` instead of `uv pip install -e .`.

Thanks for your help!

## Environment
uv 0.1.15
python 3.9.9
MacOS

---

_Comment by @charliermarsh on 2024-03-06 13:50_

Thanks -- this is by design right now as per the error message: we don't respect arbitrary direct URL dependencies, and instead require that they're declared upfront as first-party dependencies. You can track that behavior here: https://github.com/astral-sh/uv/issues/1808.


---

_Comment by @charliermarsh on 2024-03-06 13:50_

(Gonna merge into #1808.)

---

_Closed by @charliermarsh on 2024-03-06 13:50_

---

_Comment by @adrienball on 2024-03-06 14:30_

Thanks for your reactivity :)
I'll keep following for potential updates on #1808 then.
Feel free to close this issue if necessary.

---
