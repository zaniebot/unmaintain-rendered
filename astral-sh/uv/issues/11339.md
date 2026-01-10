```yaml
number: 11339
title: "Custom index: unexpected HTML error (but works in pip)"
type: issue
state: closed
author: PhilipVinc
labels:
  - compatibility
assignees: []
created_at: 2025-02-08T11:06:27Z
updated_at: 2025-02-08T15:05:05Z
url: https://github.com/astral-sh/uv/issues/11339
synced_at: 2026-01-10T03:50:31Z
```

# Custom index: unexpected HTML error (but works in pip)

---

_Issue opened by @PhilipVinc on 2025-02-08 11:06_

### Summary

Testing out the private index https://github.com/astariul/github-hosted-pypi I tried the following

```python
.venv ❯ uv --verbose add public-hello --extra-index-url https://astariul.github.io/github-hosted-pypi/
DEBUG uv 0.5.29 (ca73c4754 2025-02-05)
warning: Indexes specified via `--extra-index-url` will not be persisted to the `pyproject.toml` file; use `--index` instead.
DEBUG Found project root: `/Users/filippo.vicentini/Downloads/test`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/filippo.vicentini/Downloads/test`
DEBUG Reading Python requests from version file at `/Users/filippo.vicentini/Downloads/test/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.13 in managed installations or search path
DEBUG Searching for managed installations at `/Users/filippo.vicentini/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.1-macos-aarch64-none`
DEBUG Found `cpython-3.13.1-macos-aarch64-none` at `/Users/filippo.vicentini/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13` (managed installations)
Using CPython 3.13.1
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /Users/filippo.vicentini/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13
DEBUG Using base executable for virtual environment: /Users/filippo.vicentini/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13
DEBUG Released lock at `/var/folders/b_/dvph1c6569155bspz2m2sml40000gp/T/uv-5ddada66f7fa2d4b.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test @ file:///Users/filippo.vicentini/Downloads/test
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.1
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: test*
DEBUG Searching for a compatible version of test @ file:///Users/filippo.vicentini/Downloads/test (*)
DEBUG Adding direct dependency: public-hello*
DEBUG No cache entry for: https://astariul.github.io/github-hosted-pypi/public-hello/
DEBUG Reverting changes to `pyproject.toml`
DEBUG Removing `uv.lock`
error: Received some unexpected HTML from https://astariul.github.io/github-hosted-pypi/public-hello/
  Caused by: Unsupported hash algorithm (expected one of: `md5`, `sha256`, `sha384`, or `sha512`) on: `egg=public-hello-0.1`
```
Note that the same error is raised by `uv pip install ...`.

However, the same command with pip works
```python
.venv ❯ python -m pip install public-hello --extra-index-url https://astariul.github.io/github-hosted-pypi/
Looking in indexes: https://pypi.org/simple, https://astariul.github.io/github-hosted-pypi/
Collecting public-hello
  Cloning https://github.com/astariul/public-hello (to revision 0.2) to /private/var/folders/b_/dvph1c6569155bspz2m2sml40000gp/T/pip-install-m8x94bih/public-hello_f3c31004de084281973d42adcf66c53c
  Running command git clone --filter=blob:none --quiet https://github.com/astariul/public-hello /private/var/folders/b_/dvph1c6569155bspz2m2sml40000gp/T/pip-install-m8x94bih/public-hello_f3c31004de084281973d42adcf66c53c
  Resolved https://github.com/astariul/public-hello to commit ff01795feaf7cfa3748f489f2458e629a3190801
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting mydependency==1.0 (from public-hello)
  Cloning https://github.com/astariul/mydependency (to revision v1.0) to /private/var/folders/b_/dvph1c6569155bspz2m2sml40000gp/T/pip-install-m8x94bih/mydependency_12e09e6de46d4439a655a472869fe98b
  Running command git clone --filter=blob:none --quiet https://github.com/astariul/mydependency /private/var/folders/b_/dvph1c6569155bspz2m2sml40000gp/T/pip-install-m8x94bih/mydependency_12e09e6de46d4439a655a472869fe98b
  Resolved https://github.com/astariul/mydependency to commit 4eecc0e4d4609c18ea71332eb96e99ecd48cb7e1
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: public-hello, mydependency
  Building wheel for public-hello (pyproject.toml) ... done
  Created wheel for public-hello: filename=public_hello-0.2-py3-none-any.whl size=1613 sha256=f1587060eded56f463acc02f7974c34c69b451d5bb2538eab1690fa01d6f53fd
  Stored in directory: /private/var/folders/b_/dvph1c6569155bspz2m2sml40000gp/T/pip-ephem-wheel-cache-o1u_psyp/wheels/7a/f6/a3/db6b99b2886d4b6f33b03bf3fe3f6985dc33a4cce09d6f6085
  Building wheel for mydependency (pyproject.toml) ... done
  Created wheel for mydependency: filename=mydependency-1.0-py3-none-any.whl size=1564 sha256=15a4dab4349cc8e6d233ef6d9ba57e39ecf91e654f230c53b974a189adf7f3c5
  Stored in directory: /private/var/folders/b_/dvph1c6569155bspz2m2sml40000gp/T/pip-ephem-wheel-cache-o1u_psyp/wheels/2a/74/46/4988d2b2ef702ac4c4e86c97f066e511a1c143b2e9c85f11d8
Successfully built public-hello mydependency
Installing collected packages: mydependency, public-hello
Successfully installed mydependency-1.0 public-hello-0.2
```

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.5.29 (ca73c4754 2025-02-05)

### Python version

Python 3.13.1

---

_Label `bug` added by @PhilipVinc on 2025-02-08 11:06_

---

_Comment by @PhilipVinc on 2025-02-08 11:09_

Ah sorry, I suppose that is equivalent to https://github.com/astral-sh/uv/issues/2602 which is not planned.

However, you do support installing `uv add git+https://github.com/astariul/mydependency@v1.0#egg=mydependency-1.0` so it's a bit weird that you don't support it just in the registry...

---

_Comment by @PhilipVinc on 2025-02-08 11:13_

(Though, maybe, a more insightful error message would be helpful)

---

_Comment by @charliermarsh on 2025-02-08 13:42_

We can probably ignore `egg=` in the registry. It has no effect in uv though.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-08 13:42_

---

_Label `bug` removed by @charliermarsh on 2025-02-08 13:42_

---

_Label `compatibility` added by @charliermarsh on 2025-02-08 13:42_

---

_Closed by @charliermarsh on 2025-02-08 14:00_

---

_Closed by @charliermarsh on 2025-02-08 14:00_

---

_Comment by @PhilipVinc on 2025-02-08 15:05_

thanks!

---
