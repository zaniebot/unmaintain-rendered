```yaml
number: 17457
title: Accessing wrong g++ compiler when using uv to install the package qutip on linux redhat
type: issue
state: closed
author: BenjaminDAnjou
labels:
  - needs-mre
assignees: []
created_at: 2026-01-14T02:48:15Z
updated_at: 2026-01-15T17:37:07Z
url: https://github.com/astral-sh/uv/issues/17457
synced_at: 2026-01-15T17:50:29Z
```

# Accessing wrong g++ compiler when using uv to install the package qutip on linux redhat

---

_@BenjaminDAnjou_

### Summary

I am working on a remote server that runs Linux RedHat and I have a strange error that I'm having a hard time to pin down.

I am trying to install the [qutip](https://qutip.readthedocs.io/en/stable/) package using `uv add`:

```
cd
mkdir project
cd project
uv init --python 3.13.3
uv --verbose add qutip
```
This produces the error:

```
      [stderr]
      error: command '/usr/bin/aarch64-linux-gnu-g++' failed: No such file
      or directory

      hint: This usually indicates a problem with the package or the build
      environment.
  help: If you want to add the package regardless of the failed resolution,
        provide the `--frozen` flag to skip locking and syncing.
DEBUG Released lock at `/net/nfs-iq/home-gh/username/project/.venv/.lock`
DEBUG Released lock at `/home/username/.cache/uv/.lock`
```
It seems like uv is looking for the wrong compiler. It feels like it should be looking for

```
/usr/bin/aarch64-redhat-linux-g++
```

which is the compiler that exists on my distribution.

Weirdly, this problem only occurs for the `qutip` package. The other packages I use don't have this issue. But if I try to install `qutip` with `pyenv` and `pyenv-virtualenv` using `pip`, I have no issue at all. So there seems to be part of this that is `uv` specific.

Let me know what you think, and whether you think this is issue is more appropriately directed to the devs of `qutip`. This is a bit beyond me.

**UPDATE:** I managed to get someone else to try on the same machine, and they did not have the same issue. So there must be something in my environment that causes this issue when interacting with both `qutip` and `uv`. I will report back if I get more information.

**OS INFO:** For completeness, the output of `cat /etc/os-release` is:

```
NAME="Rocky Linux"
VERSION="9.6 (Blue Onyx)"
ID="rocky"
ID_LIKE="rhel centos fedora"
VERSION_ID="9.6"
PLATFORM_ID="platform:el9"
PRETTY_NAME="Rocky Linux 9.6 (Blue Onyx)"
ANSI_COLOR="0;32"
LOGO="fedora-logo-icon"
CPE_NAME="cpe:/o:rocky:rocky:9::baseos"
HOME_URL="https://rockylinux.org/"
VENDOR_NAME="RESF"
VENDOR_URL="https://resf.org/"
BUG_REPORT_URL="https://bugs.rockylinux.org/"
SUPPORT_END="2032-05-31"
ROCKY_SUPPORT_PRODUCT="Rocky-Linux-9"
ROCKY_SUPPORT_PRODUCT_VERSION="9.6"
REDHAT_SUPPORT_PRODUCT="Rocky Linux"
REDHAT_SUPPORT_PRODUCT_VERSION="9.6"
```

### Platform

Linux 5.14.0-427.16.1.el9_4.aarch64+64k aarch64 GNU/Linux

### Version

uv 0.9.25

### Python version

Python 3.13.3

---

_Label `bug` added by @BenjaminDAnjou on 2026-01-14 02:48_

---

_Renamed from "Error using uv to install the package qutip on linux redhat" to "Accessing wrong g++ compiler when using uv to install the package qutip on linux redhat" by @BenjaminDAnjou on 2026-01-14 02:49_

---

_Comment by @konstin on 2026-01-14 09:35_

CC @geofft @jjhelmus 

---

_Comment by @samypr100 on 2026-01-14 13:37_

> UPDATE: I managed to get someone else to try on the same machine, and they did not have the same issue. So there must be something in my environment that causes this issue when interacting with both qutip and uv. I will report back if I get more information.

@BenjaminDAnjou I also tried to repo using an isolated container but was unable to do so. See below

```shell
> docker run -it --rm rockylinux:9 bash -c 'cd; curl -LsSf https://astral.sh/uv/install.sh | sh; source .bashrc; mkdir project; cd project; uv init --python 3.13.3; uv --verbose add qutip'
Initialized project `project`
DEBUG uv 0.9.25
DEBUG Acquired shared lock for `/root/.cache/uv`
DEBUG Found project root: `/root/project`
DEBUG No workspace root found, using project root
DEBUG Acquired exclusive lock for `/root/project`
DEBUG Reading Python requests from version file at `/root/project/.python-version`
DEBUG Using Python request `3.13.3` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG Using request timeout of 30s
DEBUG Searching for Python 3.13.3 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.9.18-linux-x86_64-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from first executable in the search path: does not satisfy request `3.13.3`
DEBUG Acquired exclusive lock for `/root/.local/share/uv/python`
INFO Fetching requested Python...
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250529/cpython-3.13.3%2B20250529-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Extracting cpython-3.13.3-20250529-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /root/.local/share/uv/python/.temp/.tmpCCJcNt
Downloading cpython-3.13.3-linux-x86_64-gnu (download) (33.7MiB)
 Downloaded cpython-3.13.3-linux-x86_64-gnu (download)
DEBUG Moving /root/.local/share/uv/python/.temp/.tmpCCJcNt/python to /root/.local/share/uv/python/cpython-3.13.3-linux-x86_64-gnu
DEBUG Released lock at `/root/.local/share/uv/python/.lock`
Using CPython 3.13.3
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /root/.local/share/uv/python/cpython-3.13.3-linux-x86_64-gnu/bin/python3.13
DEBUG Using base executable for virtual environment: /root/.local/share/uv/python/cpython-3.13.3-linux-x86_64-gnu/bin/python3.13
DEBUG Released lock at `/tmp/uv-cca5eaf0b6b257de.lock`
DEBUG Acquired exclusive lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: project @ file:///root/project
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
DEBUG Adding direct dependency: project*
DEBUG Searching for a compatible version of project @ file:///root/project (*)
DEBUG Adding direct dependency: qutip*
DEBUG No cache entry for: https://pypi.org/simple/qutip/
DEBUG Searching for a compatible version of qutip (*)
DEBUG Selecting: qutip==5.2.2 [compatible] (qutip-5.2.2-cp313-cp313-macosx_10_13_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/15/04/e4e659c9b6da5d27be78a506d7b90c583eda5ab26dcb723314678c523df0/qutip-5.2.2-cp313-cp313-macosx_10_13_x86_64.whl.metadata
DEBUG Adding transitive dependency for qutip==5.2.2: numpy>=1.22
DEBUG Adding transitive dependency for qutip==5.2.2: packaging*
DEBUG Adding transitive dependency for qutip==5.2.2: scipy>=1.9, <1.16.0 | >=1.16.0+
DEBUG No cache entry for: https://pypi.org/simple/numpy/
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG No cache entry for: https://pypi.org/simple/scipy/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
WARN Skipping file for numpy: numpy-1.0.1.dev3460.win32-py2.4.exe
WARN Skipping file for numpy: numpy-1.3.0.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.5.0.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.5.1.win32-py2.5-nosse.exe
WARN Skipping file for numpy: numpy-1.5.1.win32-py2.6-nosse.exe
WARN Skipping file for numpy: numpy-1.5.1.win32-py2.7-nosse.exe
WARN Skipping file for numpy: numpy-1.5.1.win32-py3.1-nosse.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.6.0.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.6.1.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.6.2.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.7.0.win32-py3.3.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py2.5.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py2.6.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py2.7.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py3.1.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py3.2.exe
WARN Skipping file for numpy: numpy-1.7.1.win32-py3.3.exe
DEBUG Searching for a compatible version of numpy (>=1.22)
DEBUG Selecting: numpy==2.4.1 [compatible] (numpy-2.4.1-cp313-cp313-macosx_10_13_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/04/68/732d4b7811c00775f3bd522a21e8dd5a23f77eb11acdeb663e4a4ebf0ef4/numpy-2.4.1-cp313-cp313-macosx_10_13_x86_64.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/0c/51/3468fdfd49387ddefee1636f5cf6d03ce603b75205bf439bbf0e62069bfd/scipy-1.17.0-cp313-cp313-macosx_10_14_x86_64.whl.metadata
DEBUG Searching for a compatible version of packaging (*)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of scipy (>=1.9, <1.16.0 | >=1.16.0+)
DEBUG Selecting: scipy==1.17.0 [compatible] (scipy-1.17.0-cp313-cp313-macosx_10_14_x86_64.whl)
DEBUG Adding transitive dependency for scipy==1.17.0: numpy>=1.26.4, <2.7
DEBUG Tried 5 versions: numpy 1, packaging 1, project 1, qutip 1, scipy 1
DEBUG all marker environments resolution took 0.377s
Resolved 5 packages in 380ms
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: project @ file:///root/project
DEBUG No workspace root found, using project root
DEBUG Resolving despite existing lockfile due to mismatched requirements for: `project==0.1.0`
  Requested: {Requirement { name: PackageName("qutip"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "5.2.2" }]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("qutip"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
DEBUG Adding direct dependency: project*
DEBUG Searching for a compatible version of project @ file:///root/project (*)
DEBUG Adding direct dependency: qutip>=5.2.2
DEBUG Searching for a compatible version of qutip (>=5.2.2)
DEBUG Selecting: qutip==5.2.2 [preference] (qutip-5.2.2-cp313-cp313-macosx_10_13_x86_64.whl)
DEBUG Adding transitive dependency for qutip==5.2.2: numpy>=1.22
DEBUG Adding transitive dependency for qutip==5.2.2: packaging*
DEBUG Adding transitive dependency for qutip==5.2.2: scipy>=1.9, <1.16.0 | >=1.16.0+
DEBUG Searching for a compatible version of numpy (>=1.22)
DEBUG Selecting: numpy==2.4.1 [preference] (numpy-2.4.1-cp313-cp313-macosx_10_13_x86_64.whl)
DEBUG Searching for a compatible version of packaging (*)
DEBUG Selecting: packaging==25.0 [preference] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of scipy (>=1.9, <1.16.0 | >=1.16.0+)
DEBUG Selecting: scipy==1.17.0 [preference] (scipy-1.17.0-cp313-cp313-macosx_10_14_x86_64.whl)
DEBUG Adding transitive dependency for scipy==1.17.0: numpy>=1.26.4, <2.7
DEBUG Tried 5 versions: numpy 1, packaging 1, project 1, qutip 1, scipy 1
DEBUG all marker environments resolution took 0.001s
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: qutip==5.2.2
DEBUG Identified uncached distribution: numpy==2.4.1
DEBUG Identified uncached distribution: packaging==25.0
DEBUG Identified uncached distribution: scipy==1.17.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/63/1e/12fbf2a3bb240161651c94bb5cdd0eae5d4e8cc6eaeceb74ab07b12a753d/scipy-1.17.0-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/92/98/bf98e661b24387bbf559a5015341c64d04ea3e58d28eed4b05402bc33bae/qutip-5.2.2-cp313-cp313-manylinux2014_x86_64.manylinux_2_17_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ba/87/d341e519956273b39d8d47969dd1eaa1af740615394fe67d06f1efa68773/numpy-2.4.1-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl
Downloading numpy (15.6MiB)
Downloading scipy (33.4MiB)
Downloading qutip (29.9MiB)
 Downloaded numpy
 Downloaded qutip
 Downloaded scipy
Prepared 4 packages in 4.59s
Installed 4 packages in 69ms
 + numpy==2.4.1
 + packaging==25.0
 + qutip==5.2.2
 + scipy==1.17.0
DEBUG Released lock at `/root/project/.venv/.lock`
DEBUG Released lock at `/root/.cache/uv/.lock`
```

---

_Comment by @BenjaminDAnjou on 2026-01-14 14:42_

So we managed to fix the problem. What we had to do was to explicitly delete all the `uv` packets (in `~/.local/share/uv`) and the `uv` cache (in `~/.cache/uv`), and the problem was fixed.

It's hard to know exactly what happened. Maybe an interrupted process left in files that were supposed to be deleted after execution. In any case, I'll leave this here in case anyone has the same issue.

---

_Label `bug` removed by @konstin on 2026-01-14 14:43_

---

_Label `needs-mre` added by @konstin on 2026-01-14 14:43_

---

_Comment by @zanieb on 2026-01-15 17:37_

(Closing for now, but happy to open if someone else encounters this)

---

_Closed by @zanieb on 2026-01-15 17:37_

---
