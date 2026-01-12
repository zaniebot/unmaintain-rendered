```yaml
number: 14043
title: UV does not appear to respect --no-build-isolation flag or pyproject.toml build dependencies
type: issue
state: closed
author: jsmith212
labels:
  - needs-mre
assignees: []
created_at: 2025-06-15T21:10:45Z
updated_at: 2025-06-22T19:54:46Z
url: https://github.com/astral-sh/uv/issues/14043
synced_at: 2026-01-12T16:01:42Z
```

# UV does not appear to respect --no-build-isolation flag or pyproject.toml build dependencies

---

_@jsmith212_

### Summary

I am attempting to install the `slycot` package with uv in a virtual environment. Unfortunately, this requires `f2py` sources, which are normally distributed with `numpy`, but are not present in the build environment when installing. Attempting `uv add slycot --no-build-isolation` or `uv pip install slycot --no-build-isolation` produces the same error: 

```
      CMake Error at slycot/CMakeLists.txt:660 (add_library):
        Cannot find source file:

          /home/<user>/.cache/uv/builds-v0/.tmpUAW6fX/lib/python3.13/site-packages/numpy/f2py/src/fortranobject.c

        Tried extensions .c .C .c++ .cc .cpp .cxx .cu .mpp .m .M .mm .ixx .cppm .h
        .hh .h++ .hm .hpp .hxx .in .txx .f .F .for .f77 .f90 .f95 .f03 .hip .ispc

      
      CMake Error at /home/<user>/Git/gnc-rs/.venv/lib/python3.13/site-packages/skbuild/resources/cmake/FindF2PY.cmake:92 (add_library):
        No SOURCES given to target: _f2py_runtime_library
      Call Stack (most recent call first):
        CMakeLists.txt:15 (find_package)

      
      CMake Error at slycot/CMakeLists.txt:660 (add_library):
        No SOURCES given to target: _wrapper


      hint: This usually indicates a problem with the package or the build environment.
```

I have ensured all the build dependencies are installed, and I can see the file it needs in the venv lib directory. I have also attempted to follow the build isolation example in the documentation. Nothing I add to the `pyproject.toml` seems to produce any kind of change in behavior though. Is there any way to force the missing source into the build environment, or is this just impossible to install?

### Platform

Linux 6.6.87.1-microsoft-standard-WSL2+ x86_64 GNU/Linux

### Version

uv 0.7.12

### Python version

Python 3.13.4

---

_Label `bug` added by @jsmith212 on 2025-06-15 21:10_

---

_Comment by @charliermarsh on 2025-06-16 01:41_

I think this is working as expected for me?

```
uv init foo
cd foo
uv venv
uv pip install setuptools setuptools-scm scikit-build
uv add slycot --no-build-isolation
```

The build failed on my machine due to a CMAke error, but it's clearly picking up the build dependencies from the `.venv` (since if I omit `uv pip install setuptools setuptools-scm scikit-build`, I get an error that they're missing).

---

_Label `bug` removed by @charliermarsh on 2025-06-20 20:08_

---

_Label `needs-mre` added by @charliermarsh on 2025-06-20 20:08_

---

_Comment by @charliermarsh on 2025-06-20 20:08_

Closing for now but happy to re-open with more information and a reproduction.

---

_Closed by @charliermarsh on 2025-06-20 20:08_

---

_Comment by @jsmith212 on 2025-06-22 17:02_

I apologize for the delay, life got in the way this week. Here's some more info:

You're right, that's a good point that it seems to have the python build dependencies at least to some degree. Specifically the problem I'm seeing though is that the CMake error indicates a missing file: 

```
      CMake Error at slycot/CMakeLists.txt:660 (add_library):
        Cannot find source file:

          /home/smitj/.cache/uv/builds-v0/.tmpUAW6fX/lib/python3.13/site-packages/numpy/f2py/src/fortranobject.c

        Tried extensions .c .C .c++ .cc .cpp .cxx .cu .mpp .m .M .mm .ixx .cppm .h
        .hh .h++ .hm .hpp .hxx .in .txx .f .F .for .f77 .f90 .f95 .f03 .hip .ispc
```

If I look in the `.venv`, I can see that file exactly where it should be:

```
❯ ll .venv/lib/python3.13/site-packages/numpy/f2py/src
total 64K
drwxr-xr-x 2 smitj smitj 4.0K Jun 22 10:57 .
drwxr-xr-x 5 smitj smitj 4.0K Jun 22 10:57 ..
-rw-r--r-- 2 smitj smitj  46K Jun 22 10:57 fortranobject.c
-rw-r--r-- 2 smitj smitj 5.7K Jun 22 10:57 fortranobject.h
```

So that's what led me to believe that perhaps build isolation is still occurring. Not sure if that's the same build error you see or not. This one is notoriously a PITA to compile because it needs Fortran and OpenBLAS, let me see if I can get a Dockerfile that reproduces this a bit more cleanly. 

---

_Comment by @jsmith212 on 2025-06-22 19:31_

The plot thickens. I set up a Dockerfile to try to reproduce, but it actually succeeds. I've confirmed this exact sequence fails on my host machine (Ubuntu in WSL), even with the right dependencies installed. Note that this is exactly how I have UV and Python installed on my host machine, and I did try upgrading UV to 0.7.13 to rule that out. To attempt, build and run the following Dockerfile:

```docker
FROM ubuntu:latest

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    curl \
    libopenblas-dev \
    gfortran

# Install UV
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

# Add local bin to PATH
ENV PATH="/root/.local/bin:${PATH}"

# Install Python with UV, needs preview flag because UV does not officially support this yet
RUN uv python install --default --preview

WORKDIR /workspace
CMD /bin/bash
```

`docker build -t test . && docker run --rm -it test`

Run the following commands (same in Docker as on the host):

```
uv init
uv venv
uv add numpy scikit-build setuptools setuptools-scm
uv add slycot
```

This reliably works in Docker and fails on the host. I can't fault UV at this point (so no need to reopen), but I'm documenting in case anyone else runs into it or has any ideas. 

---

_Comment by @konstin on 2025-06-22 19:38_

You can try cleaning the cache:

```
uv cache clean
```

You can also look at the verbose logs (`-v` or `-vv`) to see if anything is noticeable or if there is a different between the host and the docker execution.

---

_Comment by @jsmith212 on 2025-06-22 19:54_

Great call on the cache, it now attempts to build on the host! I think that was it. I still run into an issue compiling but it appears to be an ABI issue with a dependent library, which is definitely not a UV issue. 

---
