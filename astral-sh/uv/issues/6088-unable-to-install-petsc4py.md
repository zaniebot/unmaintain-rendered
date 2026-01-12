```yaml
number: 6088
title: unable to install petsc4py
type: issue
state: closed
author: owlkward
labels:
  - bug
assignees: []
created_at: 2024-08-14T16:40:14Z
updated_at: 2025-01-09T14:59:37Z
url: https://github.com/astral-sh/uv/issues/6088
synced_at: 2026-01-12T15:59:01Z
```

# unable to install petsc4py

---

_@owlkward_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
- uv version: 0.2.36
- python version: 3.12.4
- pip version: 24.2
- petsc4py version: 3.21.4 (but I have tested a few random other versions and all of them fail the same)
- Platform: Arch Linux x86_64

All install commands where tested in a clean new virtual environment and produce the same output with added --no-cache.
The uv command that fails and its output is:
```
uv pip install "petsc4py==3.21.4"
Resolved 4 packages in 3m 11s
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: petsc4py==3.21.4
  Caused by: Failed to build: `petsc4py==3.21.4`
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_src
using Cython 3.0.11
cythonizing 'petsc4py/PETSc.pyx' -> 'petsc4py/PETSc.c'
running build_py
creating build
creating build/lib.linux-x86_64-cpython-312
creating build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/PETSc.py -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/__init__.py -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/__main__.py -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/typing.py -> build/lib.linux-x86_64-cpython-312/petsc4py
creating build/lib.linux-x86_64-cpython-312/petsc4py/lib
copying src/petsc4py/lib/__init__.py -> build/lib.linux-x86_64-cpython-312/petsc4py/lib
copying src/petsc4py/__init__.pyi -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/__main__.pyi -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/py.typed -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/PETSc.pxd -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/PETSc.h -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/PETSc_api.h -> build/lib.linux-x86_64-cpython-312/petsc4py
creating build/lib.linux-x86_64-cpython-312/petsc4py/include
creating build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/numpy.h -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/petsc4py.h -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/pybuffer.h -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/pyscalar.h -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/petsc4py.i -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/lib/__init__.pyi -> build/lib.linux-x86_64-cpython-312/petsc4py/lib
copying src/petsc4py/lib/__init__.pyi -> build/lib.linux-x86_64-cpython-312/petsc4py/lib
copying src/petsc4py/lib/petsc.cfg -> build/lib.linux-x86_64-cpython-312/petsc4py/lib
running build_ext
PETSC_DIR:    /tmp/.tmpNY57CB/builds-v0/.tmpEjCipm/lib/python3.12/site-packages/petsc
PETSC_ARCH:   
version:      3.21.4 release
integer-size: 32-bit
scalar-type:  real
precision:    double
language:     CONLY
compiler:     /usr/bin/mpicc
linker:       /usr/bin/mpicc
building 'PETSc' extension
creating build/temp.linux-x86_64-cpython-312
creating build/temp.linux-x86_64-cpython-312/src
creating build/temp.linux-x86_64-cpython-312/src/petsc4py
/usr/bin/mpicc -fPIC -Wall -Wwrite-strings -Wno-unknown-pragmas -Wno-lto-type-mismatch -Wno-stringop-overflow -fstack-protector -g -O -fPIC -fno-strict-overflow -DNDEBUG -g -O3 -Wall -march=x86-64 -mtune=generic -O3 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -g -ffile-prefix-map=/build/python/src=/usr/src/debug/python -flto=auto -ffat-lto-objects -march=x86-64 -mtune=generic -O3 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -g -ffile-prefix-map=/build/python/src=/usr/src/debug/python -flto=auto -march=x86-64 -mtune=generic -O3 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -g -ffile-prefix-map=/build/python/src=/usr/src/debug/python -flto=auto -DMPICH_SKIP_MPICXX=1 -DOMPI_SKIP_MPICXX=1 -DNPY_NO_DEPRECATED_API=NPY_1_7_API_VERSION -Isrc -Isrc/petsc4py/include -I/tmp/.tmpNY57CB/builds-v0/.tmpEjCipm/lib/python3.12/site-packages/numpy/_core/include -I/tmp/.tmpNY57CB/built-wheels-v3/pypi/petsc/3.21.4/P3e4qOhmc2TGpnFhGnpc6/petsc-3.21.4.tar.gz/build/bdist.linux-x86_64/wheel/petsc/include -I/tmp/.tmpNY57CB/builds-v0/.tmpEjCipm/include -I/usr/include/python3.12 -c src/petsc4py/PETSc.c -o build/temp.linux-x86_64-cpython-312/src/petsc4py/PETSc.o
--- stderr:
src/petsc4py/PETSc.c:1222:10: fatal error: petsc.h: No such file or directory
 1222 | #include <petsc.h>
      |          ^~~~~~~~~
compilation terminated.
error: command '/usr/bin/mpicc' failed with exit code 1
---
  Caused by: This error likely indicates that you need to install a library that provides "petsc.h" for petsc4py==3.21.4
  ```

The error looked to me like it actually fails in the petsc dependency of petsc4py, but if I install "petsc==3.21.4" separately that works, but if I try to install petsc4py afterwards I still get the same error. 
  
 When I try to install the package with pip both
```
pip install "petsc4py==3.21.4"
```
and
```
pip install "petsc4py==3.21.4" --use-pep517
```
work and adding --no-build-environment to the uv command only causes different errors, so it doesn't seem to be that issue.

---

_Comment by @charliermarsh on 2024-08-14 16:40_

What error do you get with `--no-build-environment`?

---

_Comment by @owlkward on 2024-08-14 16:57_

I had a typo there I guess, the flag is actually called `--no-build-isolation` and it turns out I was wrong, after thinking about it. The different error I meant was:
```
uv pip install --no-build-isolation "petsc4py==3.21.4"
Resolved 3 packages in 1.88s
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: petsc4py==3.21.4
  Caused by: Failed to build: `petsc4py==3.21.4`
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_src
using Cython 3.0.11
cythonizing 'petsc4py/PETSc.pyx' -> 'petsc4py/PETSc.c'
running build_py
creating build
creating build/lib.linux-x86_64-cpython-312
creating build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/PETSc.py -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/__init__.py -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/__main__.py -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/typing.py -> build/lib.linux-x86_64-cpython-312/petsc4py
creating build/lib.linux-x86_64-cpython-312/petsc4py/lib
copying src/petsc4py/lib/__init__.py -> build/lib.linux-x86_64-cpython-312/petsc4py/lib
copying src/petsc4py/__init__.pyi -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/__main__.pyi -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/py.typed -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/PETSc.pxd -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/PETSc.h -> build/lib.linux-x86_64-cpython-312/petsc4py
copying src/petsc4py/PETSc_api.h -> build/lib.linux-x86_64-cpython-312/petsc4py
creating build/lib.linux-x86_64-cpython-312/petsc4py/include
creating build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/numpy.h -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/petsc4py.h -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/pybuffer.h -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/pyscalar.h -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/petsc4py.i -> build/lib.linux-x86_64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/lib/__init__.pyi -> build/lib.linux-x86_64-cpython-312/petsc4py/lib
copying src/petsc4py/lib/__init__.pyi -> build/lib.linux-x86_64-cpython-312/petsc4py/lib
copying src/petsc4py/lib/petsc.cfg -> build/lib.linux-x86_64-cpython-312/petsc4py/lib
running build_ext
--- stderr:
PETSC_DIR not specified
error: PETSc not found
---
```
but that gets resolved when I do `uv pip install "petsc==3.21.4"` before it. Previously I only had setuptools, wheel and cython installed for the `--no-build-isolation` run, but petsc as a dependency was missing.
So `uv pip install setuptools wheel cython petsc && uv pip install petsc4py --no-build-isolation` produces the same error as directly calling `uv pip install petsc4py`

---

_Label `needs-mre` added by @charliermarsh on 2024-08-15 12:05_

---

_Label `needs-mre` removed by @charliermarsh on 2024-08-15 12:27_

---

_Label `bug` added by @charliermarsh on 2024-08-15 12:27_

---

_Comment by @charliermarsh on 2024-08-15 12:28_

I can also reproduce `pip install "petsc4py==3.21.4" --use-pep517` working, but `uv pip` failing, so I'll tag this as a bug.

---

_Comment by @charliermarsh on 2024-08-15 12:28_

It _may_ end up being a bug in the package itself, or a bug in uv, need to dig in.

---

_Comment by @wraith1995 on 2024-09-12 20:05_

I can reproduce this - I cain install fine without uv, but can't with uv. I get:

```error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: petsc4py==3.21.5
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_src
running build_py
copying src/petsc4py/PETSc.py -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/typing.py -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/__main__.py -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/lib/__init__.py -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py/lib
copying src/petsc4py/__main__.pyi -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/__init__.pyi -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/py.typed -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/PETSc.pxd -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/PETSc_api.h -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/PETSc.h -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py
copying src/petsc4py/include/petsc4py/numpy.h -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/petsc4py.h -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/pyscalar.h -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/pybuffer.h -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/include/petsc4py/petsc4py.i -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py/include/petsc4py
copying src/petsc4py/lib/__init__.pyi -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py/lib
copying src/petsc4py/lib/__init__.pyi -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py/lib
copying src/petsc4py/lib/petsc.cfg -> build/lib.macosx-11.0-arm64-cpython-312/petsc4py/lib
running build_ext
PETSC_DIR:    /Users/teoc/Library/Caches/uv/builds-v0/.tmptFGYXc/lib/python3.12/site-packages/petsc
PETSC_ARCH:   
version:      3.21.5 release
integer-size: 32-bit
scalar-type:  real
precision:    double
language:     CONLY
compiler:     gcc
linker:       gcc
building 'PETSc' extension
gcc -fPIC -Wall -Wwrite-strings -Wno-unknown-pragmas -fstack-protector -fno-stack-check -Qunused-arguments -g -O3 -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -DMPICH_SKIP_MPICXX=1 -DOMPI_SKIP_MPICXX=1 -DNPY_NO_DEPRECATED_API=NPY_1_7_API_VERSION -Isrc -Isrc/petsc4py/include -I/Users/teoc/Library/Caches/uv/builds-v0/.tmptFGYXc/lib/python3.12/site-packages/numpy/_core/include -I/Users/teoc/Library/Caches/uv/built-wheels-v3/pypi/petsc/3.21.5/w4zagi45eJEbj7BEkZ1Co/petsc-3.21.5.tar.gz/build/bdist.macosx-11.0-arm64/wheel/petsc/include -I/Users/teoc/Library/Caches/uv/builds-v0/.tmptFGYXc/include -I/Users/teoc/.local/share/uv/python/cpython-3.12.5-macos-aarch64-none/include/python3.12 -c src/petsc4py/PETSc.c -o build/temp.macosx-11.0-arm64-cpython-312/src/petsc4py/PETSc.o
--- stderr:
src/petsc4py/PETSc.c:1222:10: error: 'petsc.h' file not found with <angled> include; use "quotes" instead
#include <petsc.h>
         ^~~~~~~~~
         "petsc.h"
src/petsc4py/PETSc.c:1222:10: warning: non-portable path to file '<PETSc.h>'; specified path differs in case from file name on disk [-Wnonportable-include-path]
#include <petsc.h>
         ^~~~~~~~~
         <PETSc.h>
In file included from src/petsc4py/PETSc.c:1224:
src/lib-petsc/compat.h:4:10: fatal error: 'petsc.h' file not found
#include <petsc.h>
         ^~~~~~~~~
1 warning and 2 errors generated.
error: command '/usr/bin/gcc' failed with exit code 1
---
  Caused by: This error likely indicates that you need to install a library that provides "petsc.h" for petsc4py==3.21.5
```

Looking at the build statement, I see that this is due to `/Users/teoc/Library/Caches/uv/built-wheels-v3/pypi/petsc/3.21.5/w4zagi45eJEbj7BEkZ1Co/petsc-3.21.5.tar.gz/build/bdist.macosx-11.0-arm64/wheel/petsc/include ` not having anything in it at all.

---

_Comment by @samypr100 on 2025-01-09 03:49_

Duplicate of https://github.com/astral-sh/uv/issues/10052

---

_Closed by @zanieb on 2025-01-09 14:59_

---
