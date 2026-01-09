---
number: 15751
title: "Support portable mode: config lookup relative to uv binary & relocatable venv Python binaries"
type: issue
state: open
author: PowerWordTree
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2025-09-09T13:59:05Z
updated_at: 2025-09-16T02:02:52Z
url: https://github.com/astral-sh/uv/issues/15751
synced_at: 2026-01-07T13:12:19-06:00
---

# Support portable mode: config lookup relative to uv binary & relocatable venv Python binaries

---

_Issue opened by @PowerWordTree on 2025-09-09 13:59_

**Support portable mode: config lookup relative to uv binary & relocatable venv Python binaries**

---

**Summary**  
I’d like to propose two related features to improve uv’s portability and cross-machine usability, especially for scenarios where the installation path differs between systems (e.g., desktop on `E:\`, laptop on `C:\`, or Linux installs under `/opt`).  

---

**1. Config lookup relative to uv executable**  
- **Problem:** Currently, uv only loads configuration from project-level files, user-level config (`~/.config/uv/uv.toml` or `%APPDATA%\uv\uv.toml`), and system-level config. There is no way to have a portable uv installation that automatically uses a config file located in a path relative to the uv executable.  
- **Why it matters:**  
  - On Windows, portable installs on different drives/paths require manual environment variable setup or user-level config changes.  
  - On Linux or other systems, when uv is installed in a non-standard location, it would be convenient to ship a default config alongside the binary without touching user/system config.  
- **Proposed solution:**  
  - Add an additional, **highest-priority config lookup** step: check for a config file in a location relative to the uv executable’s directory.  
  - The exact relative path should be left to the developer or distributor; uv only needs to support resolving it relative to the binary location.

---

**2. Relocatable venv Python binaries**  
- **Problem:** uv-created virtual environments currently inherit CPython’s default behavior of embedding absolute paths into `python.exe` (Windows) or `bin/python` (Unix) and `pyvenv.cfg`. This makes the venv non-functional if the directory is moved to a different path or drive letter.  
- **Why it matters:**  
  - Portable or synchronized environments (e.g., shared between desktop and laptop, or moved between drives) break due to hardcoded paths.  
  - This is a long-standing pain point in Python’s venv mechanism, and uv could offer an opt-in improvement.  
- **Proposed solution:**  
  - Provide an option (e.g., `uv venv --relocatable`) to create venvs with relative paths in `pyvenv.cfg` and patched launchers that resolve the interpreter location at runtime.  
  - Alternatively, integrate with Python’s embeddable distribution on Windows to achieve relocatability.

---

**Benefits**  
- Enables **true portable mode** for uv without forcing a fixed directory structure.  
- Reduces friction for developers who sync environments across machines or drives.  
- Makes uv more attractive for offline, air-gapped, or USB-based workflows.  


### Example

_No response_

---

_Label `enhancement` added by @PowerWordTree on 2025-09-09 13:59_

---

_Comment by @darsor on 2025-09-15 23:37_

I'd second the need for the second point. I run into this when using devcontainers. If a virtual environment is created outside the devcontainer, it can't be used inside the container because the workspace mount point is different than the host path.

---

_Comment by @zanieb on 2025-09-16 02:02_

Regarding (1), see https://github.com/astral-sh/uv/issues/15836
Regarding (2), see https://github.com/astral-sh/uv/issues/7865

> I run into this when using devcontainers. If a virtual environment is created outside the devcontainer, it can't be used inside the container because the workspace mount point is different than the host path.

You'll usually have other problems, e.g., the packages will be installed for a different platform. I'd recommend not mounting the virtual environment into your container.

---

_Label `duplicate` added by @zanieb on 2025-09-16 02:02_

---
