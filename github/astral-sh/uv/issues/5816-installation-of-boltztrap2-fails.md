---
number: 5816
title: "Installation of `BoltzTraP2` fails"
type: issue
state: closed
author: DanielYang59
labels:
  - question
assignees: []
created_at: 2024-08-06T15:59:01Z
updated_at: 2025-02-06T10:21:53Z
url: https://github.com/astral-sh/uv/issues/5816
synced_at: 2026-01-07T13:12:17-06:00
---

# Installation of `BoltzTraP2` fails

---

_Issue opened by @DanielYang59 on 2024-08-06 15:59_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

### Summary

Hi `uv` developers! Thanks for this amazing drop-in replacement for making everything much faster. I'm having some issue installing [`BoltzTraP2`](https://gitlab.com/sousaw/BoltzTraP2) and I would appreciate your help :)


### Information 
- Python version: 3.10.12
- `uv` version: 0.2.33
- OS: Ubuntu 22.04 LTS (WSL2)

### Code to recreate

This should very easy to recreate, I could reliably recreate this failure with `uv pip install BoltzTraP2 --verbose`, and it seems `setuptools` cannot recognize `numpy` for some reason.

Meanwhile `pip install BoltzTraP2` works fine.


### Installation log

```bash
(venv) yang@Yang-NUC:~$ uv pip install -U numpy
Resolved 1 package in 2.11s
Audited 1 package in 0.02ms

(venv) yang@Yang-NUC:~$ uv pip install BoltzTraP2 --verbose
DEBUG uv 0.2.33
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/home/yang/pymatgen/venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.10.12 environment at pymatgen/venv/bin/python3
DEBUG Acquired lock for `pymatgen/venv`
DEBUG At least one requirement is not satisfied: boltztrap2
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.12
DEBUG Adding direct dependency: boltztrap2*
DEBUG Found fresh response for: https://pypi.org/simple/boltztrap2/
DEBUG Searching for a compatible version of boltztrap2 (*)
DEBUG Selecting: boltztrap2==24.7.2 [compatible] (boltztrap2-24.7.2.tar.gz)
DEBUG Acquired lock for `/home/yang/.cache/uv/built-wheels-v3/pypi/boltztrap2/24.7.2`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ff/dc/b92402e057b5b90df72f1ff611689999ae39960502a2af78f77d0f4a1f14/boltztrap2-24.7.2.tar.gz
DEBUG Preparing metadata for: boltztrap2==24.7.2
DEBUG No static `PKG-INFO` available for: boltztrap2==24.7.2 (PkgInfo(UnsupportedMetadataVersion("2.1")))
DEBUG No static `pyproject.toml` available for: boltztrap2==24.7.2 (MissingPyprojectToml)
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.10.12
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==72.1.0 [compatible] (setuptools-72.1.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e1/58/e0ef3b9974a04ce9cde2a7a33881ddcb2d68450803745804545cdd8d258f/setuptools-72.1.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG Split specific environment resolution took 0.002s
DEBUG Installing in setuptools==72.1.0 in /home/yang/.cache/uv/builds-v0/.tmpaFTDEx
DEBUG Requirement already cached: setuptools==72.1.0
DEBUG Installing build requirement: setuptools==72.1.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
error: Failed to download and build `boltztrap2==24.7.2`
  Caused by: Failed to build: `boltztrap2==24.7.2`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Error: numpy is not installed.
Please install it using your package manager or with "pip install numpy".
---
```



---

_Comment by @zanieb on 2024-08-06 16:07_

This looks like a case of https://github.com/astral-sh/uv/issues/2252

Does it fail with `pip install --use-pep517 BoltzTraP2`?

---

_Label `question` added by @zanieb on 2024-08-06 16:07_

---

_Comment by @DanielYang59 on 2024-08-06 16:09_

Thanks for the quick response, yes it does:
```bash
(venv) yang@Yang-NUC:~$ pip install --use-pep517 BoltzTraP2
Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
Collecting BoltzTraP2
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/ff/dc/b92402e057b5b90df72f1ff611689999ae39960502a2af78f77d0f4a1f14/boltztrap2-24.7.2.tar.gz (81.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 81.8/81.8 MB 12.0 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error
  
  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [2 lines of output]
      Error: numpy is not installed.
      Please install it using your package manager or with "pip install numpy".
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× Getting requirements to build wheel did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
```

---

_Comment by @zanieb on 2024-08-06 16:14_

Then the package (`BoltzTraP2`) needs to correctly specify their build dependencies or you need to use `--no-build-isolation` and install them manually e.g.

```
uv venv
source .venv/bin/activate
uv pip install numpy
uv pip install <any other build dependencies>
uv pip install BoltzTraP2 --no-build-isolation
```

---

_Referenced in [astral-sh/uv#2252](../../astral-sh/uv/issues/2252.md) on 2024-08-06 20:11_

---

_Comment by @charliermarsh on 2024-08-06 20:13_

👍 Just confirmed that this works:

```
❯ uv venv
Using Python 3.12.3 interpreter at: /Users/crmarsh/.local/share/rtx/installs/python/3.12.3/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

❯ uv pip install numpy cython setuptools
Resolved 3 packages in 6ms
Installed 3 packages in 22ms
 + cython==3.0.11
 + numpy==2.0.1
 + setuptools==72.1.0

❯ uv pip install BoltzTraP2 --no-build-isolation
Resolved 19 packages in 12ms
Installed 17 packages in 36ms
 + ase==3.23.0
 + boltztrap2==24.7.2
 + certifi==2024.7.4
 + cftime==1.6.4
 + contourpy==1.2.1
 + cycler==0.12.1
 + fonttools==4.53.1
 + kiwisolver==1.4.5
 + matplotlib==3.9.0
 + netcdf4==1.7.1.post1
 + packaging==24.1
 + pillow==10.4.0
 + pyparsing==3.1.2
 + python-dateutil==2.9.0.post0
 + scipy==1.14.0
 + six==1.16.0
 + spglib==2.5.0
```

---

_Closed by @charliermarsh on 2024-08-06 20:13_

---

_Comment by @DanielYang59 on 2024-08-07 02:38_

Thanks a lot for the information!

Update: I would expect the build dependency issue being solved for BoltzTraP2 once https://gitlab.com/sousaw/BoltzTraP2/-/merge_requests/21 is merged.

---

_Referenced in [materialsproject/pymatgen#3786](../../materialsproject/pymatgen/pulls/3786.md) on 2024-08-07 08:21_

---

_Comment by @DanielYang59 on 2025-02-05 17:50_

Hi @Lalrinkima this PR was related to the installation of `boltztrap2` from `uv` side, can you open an issue on [the `boltztrap2` repo](https://gitlab.com/sousaw/BoltzTraP2) for your issue?

---
