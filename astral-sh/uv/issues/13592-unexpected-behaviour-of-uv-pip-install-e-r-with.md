```yaml
number: 13592
title: unexpected behaviour of uv pip install -e/-r with cython extensions
type: issue
state: closed
author: schenker
labels:
  - question
assignees: []
created_at: 2025-05-22T11:25:21Z
updated_at: 2025-05-22T15:18:16Z
url: https://github.com/astral-sh/uv/issues/13592
synced_at: 2026-01-12T16:01:32Z
```

# unexpected behaviour of uv pip install -e/-r with cython extensions

---

_@schenker_

### Question

I have the following setup and I would like to understand under which conditions cython extensions are compiled:

**hello.pyx:**
```python
def say_hello():
    print("Hello from Cython!")
```

**setup.py:**
```python
from setuptools import setup, Extension

ext_modules = [
    Extension(
        name="hello",
        sources=["hello.c"],
    )
]

setup(
    name="mycythonproj",
    version="0.1",
    ext_modules=ext_modules,
)

```

**requirements.txt:**
```
-e .
```

I run `cython hello.pyx` to create `hello.c`. When I then run `uv pip install -e .`, the cython extension is compiled and `hello.cpython-312-x86_64-linux-gnu.so` is created, as expected:

```console
$ uv pip install -e .
Using Python 3.12.3 environment at: venv
Resolved 1 package in 307ms
      Built mycythonproj @ file:///home/schenker/workspace/cython_test_uv
Prepared 1 package in 722ms
Uninstalled 1 package in 0.16ms
Installed 1 package in 0.45ms
 ~ mycythonproj==0.1 (from file:///home/schenker/workspace/cython_test_uv)
```

When I instead run `uv pip install -r requirements.txt` in a fresh setup, no `.so` file is created, with the following output:
```console
$ uv pip install -r requirements.txt 
Using Python 3.12.3 environment at: venv
Resolved 1 package in 1ms
Installed 1 package in 4ms
 + mycythonproj==0.1 (from file:///home/schenker/workspace/cython_test_uv)
```


### Platform

Ubuntu 24.04.2 LTS, Linux 6.8.0-60-generic x86_64 GNU/Linux

### Version

uv 0.7.5, Python 3.12.3, Cython version 3.1.1

---

_Label `question` added by @schenker on 2025-05-22 11:25_

---

_Comment by @charliermarsh on 2025-05-22 11:34_

I believe this is because we cache the build (whereas if you explicitly install with `uv pip install -e .`, we always rebuild). See: https://docs.astral.sh/uv/concepts/cache/. You can either setup cache keys for the project, or pass `--reinstall-package`.

---

_Closed by @schenker on 2025-05-22 12:21_

---

_Comment by @schenker on 2025-05-22 15:17_

@charliermarsh thanks for the explanation! It turned out cache keys don't solve my problem. I used the .so files of my cython extension as cache keys, so they would be rebuilt if deleted. Unfortunately the rebuilt .so file's timestamp is so recent that the cache is instantly invalidated and any subsequent installs trigger a rebuild again. A file-content based key would be handy... anyways, I will try to solve it differently.

---
