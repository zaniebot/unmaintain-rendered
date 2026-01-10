---
number: 12886
title: uv init does not update .gitignore file if it already exists
type: issue
state: open
author: JerusalemProgramming
labels:
  - enhancement
assignees: []
created_at: 2025-04-14T18:55:09Z
updated_at: 2025-04-14T21:06:30Z
url: https://github.com/astral-sh/uv/issues/12886
synced_at: 2026-01-10T01:25:26Z
---

# uv init does not update .gitignore file if it already exists

---

_Issue opened by @JerusalemProgramming on 2025-04-14 18:55_

### Summary

I just discovered your company, `uv`, and `ruff` tools today.

I am interactively exploring the installation instructions.

I have noticed that if using `uv init` to initialize a project in an existing directory, then the `.gitignore` file does not get updated from using `uv init` in an existing directory vs. it does get updated with fresh info when using it in a brand, new, blank directory:

USING `uv init` IN EXISTING DIRECTORY DOES NOT UPDATE THE FOLLOWING TEXT IN `.gitignore` FILE:
```
# Created by venv; see https://docs.python.org/3/library/venv.html
*
```

USING `uv init` IN BLANK DIRECTORY PRODUCES THE FOLLOWING TEXT IN `.gitignore` FILE:
```
# Python-generated files
__pycache__/
*.py[oc]
build/
dist/
wheels/
*.egg-info

# Virtual environments
.venv
```


### Platform

Windows 10 x86_64

### Version

uv 0.6.10 (f2a2d982b 2025-03-25)

### Python version

Python 3.12.9

---

_Label `bug` added by @JerusalemProgramming on 2025-04-14 18:55_

---

_Label `enhancement` added by @Gankra on 2025-04-14 21:05_

---

_Label `bug` removed by @Gankra on 2025-04-14 21:05_

---

_Comment by @Gankra on 2025-04-14 21:06_

I could have sworn we had an issue open for this, but I can't find it. Yeah it would be nice if we did more intelligent merging/updating, or at least had an option to do so (since there's cases where it might not be correct/safe to do so).

---

_Referenced in [astral-sh/uv#12395](../../astral-sh/uv/issues/12395.md) on 2025-04-15 03:01_

---
