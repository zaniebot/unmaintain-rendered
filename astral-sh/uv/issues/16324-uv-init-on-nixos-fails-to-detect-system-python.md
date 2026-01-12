```yaml
number: 16324
title: uv init on NixOs fails to detect system python
type: issue
state: open
author: behrica
labels:
  - bug
assignees: []
created_at: 2025-10-15T19:44:38Z
updated_at: 2025-10-15T19:44:38Z
url: https://github.com/astral-sh/uv/issues/16324
synced_at: 2026-01-12T16:02:28Z
```

# uv init on NixOs fails to detect system python

---

_@behrica_

### Summary

uv init --no-managed-python -p python --verbose

DEBUG uv 0.7.22
DEBUG Searching for Python interpreter with executable name `python`
DEBUG Found `cpython-3.12.11-linux-x86_64-gnu` at `/home/carsten/.nix-profile/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/nix/store/d8wxlxpyv2h9wvb0q3f8rvqxlch9kb1n-python3-3.12.11-env/bin/python3.12`: system interpreter required
error: No interpreter found for executable name `python` in search path

'/nix/store/d8wxlxpyv2h9wvb0q3f8rvqxlch9kb1n-python3-3.12.11-env/bin/python3.12` is the working pythin, and its executed if I run "python" in shell

### Platform

NixOs

### Version

0.7.22

### Python version

Python 3.12.11

---

_Label `bug` added by @behrica on 2025-10-15 19:44_

---
