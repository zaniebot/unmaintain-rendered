```yaml
number: 15549
title: uv venv ignores UV_VENV_HOME on Windows
type: issue
state: open
author: svenviktorjonsson
labels:
  - question
assignees: []
created_at: 2025-08-27T12:25:11Z
updated_at: 2025-08-27T14:16:11Z
url: https://github.com/astral-sh/uv/issues/15549
synced_at: 2026-01-12T16:02:12Z
```

# uv venv ignores UV_VENV_HOME on Windows

---

_@svenviktorjonsson_

### Summary

## Summary

The `uv venv` command on Windows consistently ignores the `UV_VENV_HOME` environment variable. Instead of creating a virtual environment in the specified directory, it always defaults to creating a `.venv` folder in the current working directory.

---
## Steps to Reproduce

1.  Set the `UV_VENV_HOME` environment variable permanently in PowerShell:
    ```powershell
    [System.Environment]::SetEnvironmentVariable("UV_VENV_HOME", "$HOME\.virtualenvs", "User")
    ```
2.  Open a new, clean PowerShell terminal to ensure the variable is loaded.
3.  Confirm the variable is set:
    ```powershell
    echo $env:UV_VENV_HOME
    # Expected output: C:\Users\YourUser\.virtualenvs
    ```
4.  Navigate to any empty project directory.
5.  Run the environment creation command:
    ```powershell
    uv venv
    ```

---
## Expected Behavior

`uv` should create a new virtual environment inside the directory specified by `UV_VENV_HOME` (e.g., `C:\Users\YourUser\.virtualenvs\project-name`).

---
## Actual Behavior

`uv` ignores the `UV_VENV_HOME` variable and creates a `.venv` folder in the current working directory.

## Testing run

🚀 STARTING TEST: Checking if uv respects UV_VENV_HOME...
---
1. Setting $env:UV_VENV_HOME to 'C:\Users\user.name\.virtualenvs\uv-test-location'
2. Creating a temporary test directory at 'uv-temp-test-project'
3. Running `uv venv`...
Using CPython 3.11.9 interpreter at: C:\Python\Python311\python.exe
Creating virtual environment at: .venv
Activate with: .venv\Scripts\activate
4. Checking for a local '.venv' folder...
---
❌ TEST FAILED!
`uv` created a local '.venv' folder, ignoring the environment variable.
---
🧹 Cleaning up...
   - Removing temporary project directory: uv-temp-test-project
   - Unsetting $env:UV_VENV_HOME
---
✨ Test complete.
---


### Platform

Windows 10

### Version

uv 0.8.13 (ede75fe62 2025-08-21)

### Python version

Python 3.11.9

---

_Label `bug` added by @svenviktorjonsson on 2025-08-27 12:25_

---

_Comment by @svenviktorjonsson on 2025-08-27 12:27_

correction:

C:\Users\user.name.virtualenvs\uv-test-location was missing an \ and should be consistent with earlier example path:
C:\Users\YourUser\.virtualenvs\uv-test-location

---

_Comment by @zanieb on 2025-08-27 14:16_

`UV_VENV_HOME` is not something we support. Did you get that from an LLM or something..?

---

_Label `bug` removed by @zanieb on 2025-08-27 14:16_

---

_Label `question` added by @zanieb on 2025-08-27 14:16_

---
