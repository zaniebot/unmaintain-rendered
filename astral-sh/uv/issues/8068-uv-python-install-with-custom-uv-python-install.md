---
number: 8068
title: uv python install with custom UV_PYTHON_INSTALL_MIRROR is not respected for pypy
type: issue
state: closed
author: gaborbernat
labels:
  - question
assignees: []
created_at: 2024-10-10T04:57:54Z
updated_at: 2025-02-12T17:09:47Z
url: https://github.com/astral-sh/uv/issues/8068
synced_at: 2026-01-10T01:24:23Z
---

# uv python install with custom UV_PYTHON_INSTALL_MIRROR is not respected for pypy

---

_Issue opened by @gaborbernat on 2024-10-10 04:57_

```bash
❯ env UV_PYTHON_INSTALL_MIRROR=https://magic.com/py uv python install pypy3.10 -vv
    0.000351s DEBUG uv uv 0.4.18
    0.000911s DEBUG uv_fs Acquired lock for `/Users/x/Library/Application Support/uv/python`
Searching for Python versions matching: PyPy 3.10
    0.001399s DEBUG uv_client::base_client Using request timeout of 30s
 uv_python::downloads::fetch self=ManagedPythonDownload { key: PythonInstallationKey { implementation: Known(PyPy), major: 3, minor: 10, patch: 14, prerelease: "", os: Os(Darwin), arch: Arch(Aarch64(Aarch64)), libc: None }, url: "https://downloads.python.org/pypy/pypy3.10-v7.3.17-macos_arm64.tar.bz2", sha256: Some("a050e25e8d686853dd5afc363e55625165825dacfb55f8753d8225ebe417cfd2") }, download=pypy-3.10.14-macos-aarch64-none
    0.583717s 580ms DEBUG uv_python::downloads Downloading https://downloads.python.org/pypy/pypy3.10-v7.3.17-macos_arm64.tar.bz2 to temporary location: /Users/x/Library/Application Support/uv/python/.cache/.tmpklygCu
```

We should allow pypy to live on the mirror too, if the mirror does not contain, the artifact installation should fail. Otherwise, we should rename `UV_PYTHON_INSTALL_MIRROR` to `UV_CPYTHON_INSTALL_MIRROR`.

---

_Comment by @charliermarsh on 2024-10-10 09:33_

You're looking for `UV_PYPY_INSTALL_MIRROR`. It's documented in https://docs.astral.sh/uv/configuration/environment/.

---

_Label `question` added by @charliermarsh on 2024-10-10 09:33_

---

_Comment by @gaborbernat on 2024-10-10 13:31_

Any reason they need to be separated? If they must be separate, we really should rename the other one to CPython...

---

_Comment by @charliermarsh on 2024-10-10 13:34_

I think it's reasonable for them to be separate since the schemas are different. We should probably rename the other though? It pre-dated the PyPy variable.

---

_Comment by @gaborbernat on 2024-10-10 13:38_

Are these configurable via UV.toml too? Having many environment variables make it difficult to use 😅

---

_Comment by @zanieb on 2024-10-10 13:40_

I don't know if needs to be renamed, everyone says "Python" for "CPython". I think this is mostly a documentation concern?

---

_Comment by @gaborbernat on 2024-10-10 13:43_

> I don't know if needs to be renamed, everyone says "Python" for "CPython". I think this is mostly a documentation concern? 

I am not sure if i agree with you on this, but I think decisions like these are what encourage this (incorrect) behavior, that is a source of confusion for new users... And I'm not sure we should lean into that. 



---

_Referenced in [astral-sh/uv#8186](../../astral-sh/uv/issues/8186.md) on 2024-10-14 19:41_

---

_Closed by @gaborbernat on 2025-02-12 17:09_

---
