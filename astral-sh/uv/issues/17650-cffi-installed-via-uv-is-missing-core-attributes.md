```yaml
number: 17650
title: "`cffi` installed via uv is missing core attributes (`__version__` and `FFI`)"
type: issue
state: open
author: map0logo
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2026-01-21T23:14:46Z
updated_at: 2026-01-22T03:44:16Z
url: https://github.com/astral-sh/uv/issues/17650
synced_at: 2026-01-22T04:09:33Z
```

# `cffi` installed via uv is missing core attributes (`__version__` and `FFI`)

---

_@map0logo_

### Problem Description

When installing `cffi` using `uv`, the resulting package is incomplete or corrupted, missing essential attributes. This causes `AttributeError` when trying to access `cffi.__version__` or `cffi.FFI()`. The same package installed via `pip` works correctly.

### Steps to Reproduce
 
1. Create a new project directory and initialize with uv:
   ```bash
   uv init test-cffi && cd test-cffi
   ```

2. Install `cffi` using uv:
   ```bash
   uv add cffi # or uv pip install cffi
   ```

3. Activate the virtual environment and test:
   ```bash
   source .venv/bin/activate
   python -c "import cffi; print(cffi.__version__); print(cffi.FFI())"
   ```

**Actual Result:** `AttributeError` for both `__version__` and `FFI`.

```python
>>> import cffi
>>> cffi.__version__
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: module 'cffi' has no attribute '__version__'
>>> cffi.FFI()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: module 'cffi' has no attribute 'FFI'
```

**Expected Result:** Version string printed and FFI object created successfully (as happens when installed with `pip`).

### Workaround

```bash
python -m venv .venv
source .venv/bin/activate
pip install cffi
```

```python
>>> import cffi
>>> cffi.__version__
'2.0.0'
>>> cffi.FFI()
<cffi.api.FFI object at 0x7f38075cc6e0>
```

### Additional Information
This appears to be a packaging/installation issue specific to `uv`, as the standard `pip` installation works correctly. The problem may be related to how `uv` handles the package's C extensions or metadata.


### Platform

Linux 6.6.119-1-MANJARO x86_64 GNU/Linux

### Version

0.9.26

### Python version

3.12.10 

---

_Label `bug` added by @map0logo on 2026-01-21 23:14_

---

_Comment by @map0logo on 2026-01-22 02:25_


This is not the case at least in Manjaro Linux and Python 3.12

Trying your script:

```shell
uv init test-cffi && cd test-cffi
uv add cffi
echo "# shadow" > cffi.py
uv run python -c "import cffi; print(cffi.__version__)"
```

```python
Traceback (most recent call last):
  File "<string>", line 1, in <module>
AttributeError: module 'cffi' has no attribute '__version__'
```

Without shadowing

```shell
rm cffi.py
uv run python -c "import cffi; print(cffi.__version__)"
```

I got, again

```python
Traceback (most recent call last):
  File "<string>", line 1, in <module>
AttributeError: module 'cffi' has no attribute '__version__'
```



---

_Comment by @zanieb on 2026-01-22 03:40_

The problem is that this isn't trivially reproducible

```
❯ docker run -t astral/uv:bookworm bash -c 'uv init -p 3.12 test-cffi && cd test-cffi && uv add cffi && uv run -p 3.12 python -c "import cffi; print(cffi.__version__); print(cffi.FFI())"'
Initialized project `test-cffi` at `/test-cffi`
Using CPython 3.12.12
Creating virtual environment at: .venv
Resolved 3 packages in 264ms
Prepared 2 packages in 144ms
Installed 2 packages in 1ms
 + cffi==2.0.0
 + pycparser==3.0
2.0.0
<cffi.api.FFI object at 0xffff9e483560>
```

Can you reproduce in a container for us?

---

_Label `needs-mre` added by @zanieb on 2026-01-22 03:40_

---

_Comment by @zanieb on 2026-01-22 03:44_

```
❯ cat Dockerfile
FROM archlinux:latest
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/
WORKDIR /app
RUN uv init -p 3.12 test-cffi
WORKDIR /app/test-cffi
RUN uv add cffi
CMD ["uv", "run", "-p", "3.12", "python", "-c", "import cffi; print(cffi.__version__); print(cffi.FFI())"]
❯ docker build --platform linux/amd64 -t test-cffi-arch .
[+] Building 0.2s (12/12) FINISHED                                                       docker:orbstack
 => [internal] load build definition from Dockerfile                                                0.0s
 => => transferring dockerfile: 305B                                                                0.0s
 => [internal] load metadata for ghcr.io/astral-sh/uv:latest                                        0.1s
 => [internal] load metadata for docker.io/library/archlinux:latest                                 0.1s
 => [internal] load .dockerignore                                                                   0.0s
 => => transferring context: 2B                                                                     0.0s
 => FROM ghcr.io/astral-sh/uv:latest@sha256:9a23023be68b2ed09750ae636228e903a54a05ea56ed03a934d00f  0.0s
 => [stage-0 1/6] FROM docker.io/library/archlinux:latest@sha256:f5add4183c5f05abf3b65489447aee6d5  0.0s
 => CACHED [stage-0 2/6] COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/                     0.0s
 => CACHED [stage-0 3/6] WORKDIR /app                                                               0.0s
 => CACHED [stage-0 4/6] RUN uv init -p 3.12 test-cffi                                              0.0s
 => CACHED [stage-0 5/6] WORKDIR /app/test-cffi                                                     0.0s
 => CACHED [stage-0 6/6] RUN uv add cffi                                                            0.0s
 => exporting to image                                                                              0.0s
 => => exporting layers                                                                             0.0s
 => => writing image sha256:b14a099148828a240a84faa3b949fc7bd8baa184213e7cf45a5e28ab5ba85c0f        0.0s
 => => naming to docker.io/library/test-cffi-arch                                                   0.0s
❯ docker run --platform linux/amd64 -t  test-cffi-arch
2.0.0
<cffi.api.FFI object at 0x7fffff833410>
```

---
