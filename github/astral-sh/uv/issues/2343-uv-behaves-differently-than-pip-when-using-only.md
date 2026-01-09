---
number: 2343
title: "`uv` behaves differently than `pip` when using `--only-binary=:all:` for editable installs"
type: issue
state: closed
author: rrueth
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-03-10T17:48:10Z
updated_at: 2024-03-12T23:21:42Z
url: https://github.com/astral-sh/uv/issues/2343
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv` behaves differently than `pip` when using `--only-binary=:all:` for editable installs

---

_Issue opened by @rrueth on 2024-03-10 17:48_

`uv` version:
```
uv --version
uv 0.1.16 (9f1452cb7 2024-03-07)
```

I have the following package:
- pkg/
  - src/pkg/__init__.py
  - tests
  - pyproject.toml

The `pyproject.toml` contains the following:
```
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[project]
name = "pkg"
version = "1.0.0"
dependencies = []
```

I create a virtual environment with the following:
```
uv venv --python python3.11 --seed env
```

Then, if I try to install my package as an editable install, while specifying the `--only-binary=:all:` flag, it fails:
```
VIRTUAL_ENV=env uv pip install --only-binary=:all: -e ./
error: Failed to build editables
  Caused by: Failed to build editable: file:///Users/ryan/pkg
  Caused by: Building source distributions is disabled
```

Running the same command directly with `pip` succeeds:
```
./env/bin/pip install --only-binary=:all: -e ./
Obtaining file:///Users/ryan/pkg
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Installing backend dependencies ... done
  Preparing editable metadata (pyproject.toml) ... done
Building wheels for collected packages: pkg
  Building editable for pkg (pyproject.toml) ... done
  Created wheel for pkg: filename=pkg-1.0.0-0.editable-py3-none-any.whl size=1120 sha256=9b3f829a419131d6ec58795d1d4d5fad5c58c50ef24f64fad219d5f01e426fb1
  Stored in directory: /private/var/folders/qx/q6hdl8rn3x57stn_m6sy3_tm0000gn/T/pip-ephem-wheel-cache-igev1641/wheels/b0/9a/a6/2db658aee67947ede4e7a4e42146f43316514a2f3ec0e5ac7d
Successfully built pkg
Installing collected packages: pkg
Successfully installed pkg-1.0.0
```

The `pip` version is:
```
./env/bin/pip --version
pip 24.0 from /Users/ryan/pkg/env/lib/python3.11/site-packages/pip (python 3.11)
```

Our use case is that we would like to use wheels to install all of our third-party packages, but we would like to install all of our local packages as editable installs for easier development. With `pip` the `--only-binary=:all:` worked well for this case since editable packages seemed to be ignored when considering the only-binary requirement. `pip` behaves the same when editable installs are discovered in `requirements.txt` files. `uv` fails the install in the same way when detecting editable installs in the `requirements.txt` files. 


---

_Comment by @zanieb on 2024-03-11 04:13_

Hm okay thanks! I think an exception for this makes sense, it might be kind of a pain to implement though.

---

_Label `bug` added by @zanieb on 2024-03-11 04:13_

---

_Label `compatibility` added by @zanieb on 2024-03-11 04:13_

---

_Comment by @rrueth on 2024-03-12 18:47_

Thank you, @zanieb. It's been great integrating `uv` into our build system so far. I appreciate all the work you and the team are doing, the rapid responses to questions and issues, and the quick releases as things get fixed.

---

_Assigned to @zanieb by @zanieb on 2024-03-12 19:16_

---

_Referenced in [astral-sh/uv#2393](../../astral-sh/uv/pulls/2393.md) on 2024-03-12 20:58_

---

_Comment by @zanieb on 2024-03-12 20:58_

Good news, this was just an oversight in a safeguard and this is very simple to fix https://github.com/astral-sh/uv/pull/2393

And thanks for the kind words <3

---

_Closed by @zanieb on 2024-03-12 23:21_

---
