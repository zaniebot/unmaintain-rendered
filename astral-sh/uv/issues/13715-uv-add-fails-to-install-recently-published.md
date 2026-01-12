```yaml
number: 13715
title: uv add fails to install recently published package version from PyPI
type: issue
state: closed
author: DamyanBG
labels:
  - question
assignees: []
created_at: 2025-05-29T09:31:02Z
updated_at: 2025-05-29T12:38:21Z
url: https://github.com/astral-sh/uv/issues/13715
synced_at: 2026-01-12T16:01:35Z
```

# uv add fails to install recently published package version from PyPI

---

_@DamyanBG_

### Summary

After publishing a new version (0.2.0) of my Python package modular-mcp to PyPI, uv add modular-mcp==0.2.0 fails with an error saying the version does not exist:

× No solution found when resolving dependencies:
╰─▶ Because there is no version of modular-mcp==0.2.0 and your project depends on modular-mcp==0.2.0, we can conclude that your project's requirements are unsatisfiable.
However, the package is live and installable using pip without issue.

Expected Behavior
uv should detect and install the newly published version from PyPI, just like pip.

Actual Behavior
uv fails to recognize the existence of the new version for a period of time after publication.

Steps to Reproduce

Publish a new version of a package to PyPI (e.g., modular-mcp==0.2.0)

Run: uv add modular-mcp==0.2.0

Observe the version resolution failure

### Platform

Windows 11

### Version

uv 0.6.6 (c1a0bb85e 2025-03-12)

### Python version

Python 3.13.2

---

_Label `bug` added by @DamyanBG on 2025-05-29 09:31_

---

_Comment by @blueraft on 2025-05-29 09:49_

An earlier response from pypi might have been cached, does it work if you try with the no-cache flag. `uv add modular-mcp --no-cache`? 

---

_Comment by @zanieb on 2025-05-29 12:38_

There's a 10 minute cache, it sounds like you want `--refresh-package modular-mcp`

---

_Closed by @zanieb on 2025-05-29 12:38_

---

_Label `bug` removed by @zanieb on 2025-05-29 12:38_

---

_Label `question` added by @zanieb on 2025-05-29 12:38_

---
