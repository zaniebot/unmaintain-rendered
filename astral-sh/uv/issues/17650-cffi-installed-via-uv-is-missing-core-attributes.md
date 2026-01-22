```yaml
number: 17650
title: "`cffi` installed via uv is missing core attributes (`__version__` and `FFI`)"
type: issue
state: open
author: map0logo
labels:
  - bug
assignees: []
created_at: 2026-01-21T23:14:46Z
updated_at: 2026-01-21T23:15:02Z
url: https://github.com/astral-sh/uv/issues/17650
synced_at: 2026-01-22T00:09:02Z
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
