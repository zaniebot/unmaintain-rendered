---
number: 14011
title: "`uv add` not respecting pinned package version"
type: issue
state: open
author: seh-dev
labels:
  - question
assignees: []
created_at: 2025-06-12T22:51:42Z
updated_at: 2025-06-13T17:14:42Z
url: https://github.com/astral-sh/uv/issues/14011
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv add` not respecting pinned package version

---

_Issue opened by @seh-dev on 2025-06-12 22:51_

### Summary

Summary:
despite having a pinned `numpy==2.2.3` version defined in our `pyproject.toml` and `uv.lock` file, when a new package with an unbounded `numpy>=1.11.0` dependency version is specified, `uv` still appears to be trying to retrieve the latest `numpy==2.3.0` version which has no wheels available on AL2 (corresponding numpy issue: https://github.com/numpy/numpy/issues/29150)

for whatever reason `uv` tries to `build` `numpy==2.3.0`, which fails due to an incompatible `gcc` version, resulting in the whole `add` or `sync` operation to fail prematurely, despite the pinned `2.2.3` version existing and being a valid version for the project.

#### Behavior
running 
```bash
uv add numpy==2.2.3
uv add reverse-geocoder==1.5.1
```
fails because numpy `2.3.0` can't be built.

#### Expected Behavior
The operation uses the pinned version of `numpy`, or at least falls back gracefully to this version if building a newer version fails

Here's the MRE using the AL2 docker file:
```bash
$ docker run -it --rm amazonlinux:2 /bin/bash
# yum update -y
# yum install -y tar gzip gcc clang python3-pip
# curl -LsSf https://astral.sh/uv/install.sh | sh
# uv --version
# # uv 0.7.12
# mkdir test && cd test
# uv init
# uv add numpy==2.2.3
# uv add reverse-geocoder==1.5.1
  x Failed to download and build `reverse-geocoder==1.5.1`
  |-> Failed to install requirements from `build-system.requires`
  |-> Failed to build `numpy==2.3.0`
  |-> The build backend returned an error
  `-> Call to `mesonpy.build_wheel` failed (exit status: 1)

      [stdout]
      + /root/.cache/uv/builds-v0/.tmp8TTmGH/bin/python /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/vendored-meson/meson/meson.py setup
      /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/.mesonpy-dksjqaqm -Dbuildtype=release
      -Db_ndebug=if-release -Db_vscrt=md --native-file=/root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/.mesonpy-dksjqaqm/meson-python-native-file.ini
      The Meson build system
      Version: 1.6.1
      Source dir: /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src
      Build dir: /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/.mesonpy-dksjqaqm
      Build type: native build
      Project name: NumPy
      Project version: 2.3.0
      C compiler for the host machine: cc (gcc 7.3.1 "cc (GCC) 7.3.1 20180712 (Red Hat 7.3.1-17)")
      C linker for the host machine: cc ld.bfd 2.29.1-31
      C++ compiler for the host machine: c++ (gcc 7.3.1 "c++ (GCC) 7.3.1 20180712 (Red Hat 7.3.1-17)")
      C++ linker for the host machine: c++ ld.bfd 2.29.1-31
      Cython compiler for the host machine: cython (cython 3.1.2)
      Host machine cpu family: x86_64
      Host machine cpu: x86_64

      ../meson.build:28:4: ERROR: Problem encountered: NumPy requires GCC >= 9.3

      A full log can be found at /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/.mesonpy-dksjqaqm/meson-logs/meson-log.txt

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

### Additional Information
Installing `reverse-geocoder` via `pip` works, but via `uv pip` does not.
```bash
# pip3 install reverse-geocoder
WARNING: Running pip install with root privileges is generally not a good idea. Try `pip3 install --user` instead.
Collecting reverse-geocoder
  Downloading reverse_geocoder-1.5.1.tar.gz (2.2 MB)
     |████████████████████████████████| 2.2 MB 5.8 MB/s 
Collecting numpy>=1.11.0
  Using cached numpy-1.21.6-cp37-cp37m-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (15.7 MB)
Collecting scipy>=0.17.1
  Downloading scipy-1.7.3-cp37-cp37m-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (38.1 MB)
     |████████████████████████████████| 38.1 MB 24.2 MB/s 
Using legacy 'setup.py install' for reverse-geocoder, since package 'wheel' is not installed.
Installing collected packages: numpy, scipy, reverse-geocoder
    Running setup.py install for reverse-geocoder ... done
Successfully installed numpy-1.21.6 reverse-geocoder-1.5.1 scipy-1.7.3
```

```bash
# uv pip install reverse-geocoder
  x Failed to download and build `reverse-geocoder==1.5.1`
  |-> Failed to install requirements from `build-system.requires`
  |-> Failed to build `numpy==2.3.0`
  |-> The build backend returned an error
  `-> Call to `mesonpy.build_wheel` failed (exit status: 1)

      [stdout]
      + /root/.cache/uv/builds-v0/.tmpKI9YfE/bin/python /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/vendored-meson/meson/meson.py setup
      /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/.mesonpy-mzm0i2zc -Dbuildtype=release
      -Db_ndebug=if-release -Db_vscrt=md --native-file=/root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/.mesonpy-mzm0i2zc/meson-python-native-file.ini
      The Meson build system
      Version: 1.6.1
      Source dir: /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src
      Build dir: /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/.mesonpy-mzm0i2zc
      Build type: native build
      Project name: NumPy
      Project version: 2.3.0
      C compiler for the host machine: cc (gcc 7.3.1 "cc (GCC) 7.3.1 20180712 (Red Hat 7.3.1-17)")
      C linker for the host machine: cc ld.bfd 2.29.1-31
      C++ compiler for the host machine: c++ (gcc 7.3.1 "c++ (GCC) 7.3.1 20180712 (Red Hat 7.3.1-17)")
      C++ linker for the host machine: c++ ld.bfd 2.29.1-31
      Cython compiler for the host machine: cython (cython 3.1.2)
      Host machine cpu family: x86_64
      Host machine cpu: x86_64

      ../meson.build:28:4: ERROR: Problem encountered: NumPy requires GCC >= 9.3

      A full log can be found at /root/.cache/uv/sdists-v9/pypi/numpy/2.3.0/A-oyixwjPXghkYgyLarjK/src/.mesonpy-mzm0i2zc/meson-logs/meson-log.txt

      hint: This usually indicates a problem with the package or the build environment.
```
### Platform

Amazon Linux 2 (Linux 6.8.0-1029-aws x86_64 GNU/Linux)

### Version

0.7.12

### Python version

3.13.4

---

_Label `bug` added by @seh-dev on 2025-06-12 22:51_

---

_Comment by @zanieb on 2025-06-12 23:01_

```
  x Failed to download and build `reverse-geocoder==1.5.1`
  |-> Failed to install requirements from `build-system.requires`
  |-> Failed to build `numpy==2.3.0`
```

It looks like `reverse-geocoder` requires `numpy` as a build requirement, which is installed in an isolated environment to build the parent package so the resolver does not consider the dependencies of your project.

You could try to apply a constraint on the build dependency, to prefer a different numpy version https://docs.astral.sh/uv/reference/settings/#build-constraint-dependencies

---

_Comment by @zanieb on 2025-06-12 23:02_

It looks like that would work, since they don't pin a particular numpy version in their build requires https://inspector.pypi.io/project/reverse-geocoder/1.5.1/packages/0b/0f/b7d5d4b36553731f11983e19e1813a1059ad0732c5162c01b3220c927d31/reverse_geocoder-1.5.1.tar.gz/reverse_geocoder-1.5.1/setup.py#line.27

---

_Comment by @zanieb on 2025-06-12 23:03_

We're working on preferring build dependencies that are coherent with your project dependencies, but it's complicated.

---

_Label `bug` removed by @zanieb on 2025-06-12 23:05_

---

_Label `question` added by @zanieb on 2025-06-12 23:05_

---

_Comment by @seh-dev on 2025-06-13 17:14_

Thanks for all the information! That should work as a workaround for our current case. 

If graceful fallbacks on build-errors or a more direct solution suing the project dependencies are too complicated, I still might humbly suggest adding a log if there's a build error to have the user consider adding build-constraint-dependency docs; in this case, the suggested `--frozen` flag didn't help resolve the issue or lead to any relevant docs that could have helped diagnose and fix the issue independently.

---
