```yaml
number: 530
title: "`ty` reports unresolved import for `win32gui` despite successful import in Python"
type: issue
state: closed
author: ooopus
labels:
  - needs-info
  - imports
assignees: []
created_at: 2025-05-28T08:55:24Z
updated_at: 2025-05-28T10:54:59Z
url: https://github.com/astral-sh/ty/issues/530
synced_at: 2026-01-10T02:34:10Z
```

# `ty` reports unresolved import for `win32gui` despite successful import in Python

---

_Issue opened by @ooopus on 2025-05-28 08:55_

### Summary

`ty check` reports an `unresolved-import` error for the `win32gui` module, even though the module can be successfully imported and used in a Python environment where `pywin32` is installed.

**Steps to Reproduce**

1.  Install `pywin32`:
    ```bash
    pip install pywin32
    ```
2.  Create a Python file (e.g., `test.py`) with the following content:
    ```python
    import win32gui
    print("win32gui imported successfully")
    ```
3.  Verify that the Python script runs successfully and `win32gui` can be imported:
    ```bash
    python test.py
    ```
    Output:
    ```
    win32gui imported successfully
    ```
4.  Run `ty check` on the Python file:
    ```bash
    uvx ty check test.py
    ```

**Expected Behavior**

`ty check` should not report any errors, as `win32gui` is a valid and importable module after installing `pywin32`.

**Actual Behavior**

`ty check` reports an `unresolved-import` error:

```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-import]: Cannot resolve imported module `win32gui`
 --> test.py:1:8
  |
1 | import win32gui
  |        ^^^^^^^^
  |
info: make sure your Python environment is properly configured: https://github.com/astral-sh/ty/blob/main/docs/README.md#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.7 (afb20f6fe 2025-05-26)

---

_Label `imports` added by @AlexWaygood on 2025-05-28 09:10_

---

_Comment by @AlexWaygood on 2025-05-28 09:12_

Thanks for the report! Can I double-check two things:
1. Which environment are you installing `pywin32` into -- a virtual environment or your system installation of Python?
2. Do you have a virtual environment locally (if so, what directory is it in and have you activated it)?

---

_Label `needs-info` added by @MichaReiser on 2025-05-28 09:21_

---

_Comment by @ooopus on 2025-05-28 09:37_

> Thanks for the report! Can I double-check two things:
> 
> 1. Which environment are you installing `pywin32` into -- a virtual environment or your system installation of Python?
> 2. Do you have a virtual environment locally (if so, what directory is it in and have you activated it)?

1. I have installed pywin32 in both the virtual environment and the system environment
2. An error occurs regardless of whether the virtual environment (.venv) is activated.

---

_Comment by @AlexWaygood on 2025-05-28 10:11_

Ohhh, I think `win32gui` might be a C extension, if I remember correctly? You might need to install `types-pywin32` for us to be able to resolve the import. If that's the case, then this is #487.

---

_Comment by @ooopus on 2025-05-28 10:53_

Indeed, that's so. Thank you!

---

_Closed by @ooopus on 2025-05-28 10:53_

---

_Comment by @AlexWaygood on 2025-05-28 10:54_

No problem. Sorry our diagnostics aren't better here. We'll definitely try to improve this.

---
