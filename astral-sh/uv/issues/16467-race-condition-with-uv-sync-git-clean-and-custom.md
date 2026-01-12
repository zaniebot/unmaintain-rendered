```yaml
number: 16467
title: "Race Condition with `uv sync` `git clean` and custom build steps (like setuptools-scm)"
type: issue
state: open
author: schlamar
labels:
  - bug
assignees: []
created_at: 2025-10-27T08:30:42Z
updated_at: 2025-10-27T18:31:27Z
url: https://github.com/astral-sh/uv/issues/16467
synced_at: 2026-01-12T16:02:32Z
```

# Race Condition with `uv sync` `git clean` and custom build steps (like setuptools-scm)

---

_@schlamar_

### Summary

This is somehow related to #15224 but quite a different issue.

When using custom build steps (like generating a version file with setuptools-scm) the caching from uv is too aggressive for editable installs if you clean up the project directory (for example with `git clean -fxd`).

Attached a minimal example. Steps to reproduce.

```
git init
git add .
git commit -m "init"
uv sync
# activate venv & .\.venv\Scripts\Activate.ps1
pytest
git clean -fxd
uv sync
pytest 
```

In the second `uv sync` call the setuptools-scm build does not trigger and and the _version.py file is missing.

[min.zip](https://github.com/user-attachments/files/23159885/min.zip)

### Platform

Windows 11

### Version

uv 0.9.5 (d5f39331a 2025-10-21)

### Python version

Python 3.13.6


### Workaround

```
git clean -fxd
uv sync --no-cache
```

---

_Label `bug` added by @schlamar on 2025-10-27 08:30_

---

_Comment by @konstin on 2025-10-27 18:31_

`git clean -fxd` and editable installs with generated files don't really interact well, I don't really have a great idea for this case.

A more performant workaround is:
```
uv sync --reinstall-package <library>
```

---
