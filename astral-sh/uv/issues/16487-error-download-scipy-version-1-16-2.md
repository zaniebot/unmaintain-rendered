```yaml
number: 16487
title: Error download scipy version 1.16.2
type: issue
state: open
author: intexcor
labels:
  - needs-mre
assignees: []
created_at: 2025-10-28T18:20:22Z
updated_at: 2025-10-28T23:39:14Z
url: https://github.com/astral-sh/uv/issues/16487
synced_at: 2026-01-12T16:02:32Z
```

# Error download scipy version 1.16.2

---

_@intexcor_

### Summary

Error install

pet1@h200:~/social_media_analytics$ uv add scipy
Resolved 246 packages in 1.73s
Built src @ file:///home/pet1/social_media_analytics
× Failed to download scipy==1.16.2
├─▶ Failed to fetch: https://pypi.anaconda.org/mgorny/simple/scipy/1.16.2/scipy-1.16.2-cp313-cp313-linux_x86_64-x8664v4_mkl.whl
╰─▶ HTTP status client error (404 Not Found) for url (https://pypi.anaconda.org/mgorny/simple/scipy/1.16.2/scipy-1.16.2-cp313-cp313-linux_x86_64-x8664v4_mkl.whl)
help: If you want to add the package regardless of the failed resolution, provide the --frozen flag to skip locking and syncing.

Reproducing Code Example
uv add scipy
Error message
Resolved 246 packages in 1.73s
 Built src @ file:///home/pet1/social_media_analytics
 × Failed to download `scipy==1.16.2`
 ├─▶ Failed to fetch: `https://pypi.anaconda.org/mgorny/simple/scipy/1.16.2/scipy-1.16.2-cp313-cp313-linux_x86_64-x8664v4_mkl.whl`
 ╰─▶ HTTP status client error (404 Not Found) for url (https://pypi.anaconda.org/mgorny/simple/scipy/1.16.2/scipy-1.16.2-cp313-cp313-linux_x86_64-x8664v4_mkl.whl)
 help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
SciPy/NumPy/Python version and system information
1.16.1 2.3.4 sys.version_info(major=3, minor=13, micro=9, releaselevel='final', serial=0)
Build Dependencies:
 blas:
 detection method: pkgconfig
 found: true
 include directory: /opt/_internal/cpython-3.13.5/lib/python3.13/site-packages/scipy_openblas32/include
 lib directory: /opt/_internal/cpython-3.13.5/lib/python3.13/site-packages/scipy_openblas32/lib
 name: scipy-openblas
 openblas configuration: OpenBLAS 0.3.28 DYNAMIC_ARCH NO_AFFINITY Haswell MAX_THREADS=64
 pc file directory: /project
 version: 0.3.28
 lapack:
 detection method: pkgconfig
 found: true
 include directory: /opt/_internal/cpython-3.13.5/lib/python3.13/site-packages/scipy_openblas32/include
 lib directory: /opt/_internal/cpython-3.13.5/lib/python3.13/site-packages/scipy_openblas32/lib
 name: scipy-openblas
 openblas configuration: OpenBLAS 0.3.28 DYNAMIC_ARCH NO_AFFINITY Haswell MAX_THREADS=64
 pc file directory: /project
 version: 0.3.28
 pybind11:
 detection method: config-tool
 include directory: unknown
 name: pybind11
 version: 3.0.0
Compilers:
 c:
 commands: cc
 linker: ld.bfd
 name: gcc
 version: 10.2.1
 c++:
 commands: c++
 linker: ld.bfd
 name: gcc
 version: 10.2.1
 cython:
 commands: cython
 linker: cython
 name: cython
 version: 3.1.2
 fortran:
 commands: gfortran
 linker: ld.bfd
 name: gcc
 version: 10.2.1
 pythran:
 include directory: ../../tmp/build-env-0fmy4m5d/lib/python3.13/site-packages/pythran
 version: 0.18.0
Machine Information:
 build:
 cpu: x86_64
 endian: little
 family: x86_64
 system: linux
 cross-compiled: false
 host:
 cpu: x86_64
 endian: little
 family: x86_64
 system: linux
Python Information:
 path: /tmp/build-env-0fmy4m5d/bin/python
 version: '3.13'

### Platform

Linux

### Version

last

### Python version

3.13

---

_Label `bug` added by @intexcor on 2025-10-28 18:20_

---

_Comment by @intexcor on 2025-10-28 21:59_

https://github.com/scipy/scipy/issues/23857#issuecomment-3444602031

---

_Comment by @charliermarsh on 2025-10-28 23:35_

It looks like you've configured uv to look at https://pypi.anaconda.org/mgorny. Is that intentional? It seems like a mistake.

---

_Label `bug` removed by @charliermarsh on 2025-10-28 23:35_

---

_Label `needs-mre` added by @charliermarsh on 2025-10-28 23:35_

---

_Comment by @intexcor on 2025-10-28 23:37_

It's not intentional.

---

_Comment by @charliermarsh on 2025-10-28 23:39_

Do you have any such configuration in your `uv.toml` or `pyproject.toml`, or `~/.config/uv/uv.toml`? (https://docs.astral.sh/uv/concepts/configuration-files/)

---
