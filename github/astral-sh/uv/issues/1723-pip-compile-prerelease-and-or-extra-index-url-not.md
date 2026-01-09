---
number: 1723
title: "pip compile: --prerelease and/or --extra-index-url not getting correct dependency version"
type: issue
state: closed
author: sebslight
labels: []
assignees: []
created_at: 2024-02-20T00:26:00Z
updated_at: 2024-02-20T05:14:28Z
url: https://github.com/astral-sh/uv/issues/1723
synced_at: 2026-01-07T13:12:16-06:00
---

# pip compile: --prerelease and/or --extra-index-url not getting correct dependency version

---

_Issue opened by @sebslight on 2024-02-20 00:26_



Using `uv pip compile`  with `--extra-index-url=https://download.pytorch.org/whl/nightly/cu121` with `--prerelease=allow` does not locate PyTorch 2.3.0, but using `--index-url` does.  
```bash
❯ uv --version
uv 0.1.5

# Doesn't work
❯ VIRTUAL_ENV=.venv uv pip compile --no-deps --no-header --python-version 3.11.7 --extra-index-url=https://download.pytorch.org/whl/nightly/cu121 --prerelease=allow  -
torch
Resolved 1 package in 2.11s
torch==2.2.0

# Works but...
❯ VIRTUAL_ENV=.venv uv pip compile --no-deps --no-header --python-version 3.11.7 --index-url=https://download.pytorch.org/whl/nightly/cu121 --prerelease=allow  -
torch
Resolved 1 package in 1.61s
torch==2.3.0.dev20240219+cu121

# ... wouldn't work for this
❯ VIRTUAL_ENV=.venv uv pip compile --no-deps --no-header --python-version 3.11.7 --index-url=https://download.pytorch.org/whl/nightly/cu121 --prerelease=allow  -
torch
ruff
error: HTTP status client error (403 Forbidden) for url (https://download.pytorch.org/whl/nightly/cu121/ruff/)

# explicit version using --extra-index-url also doesn't work
❯ VIRTUAL_ENV=.venv uv pip compile --no-deps --no-header --python-version 3.11.7 --extra-index-url=https://download.pytorch.org/whl/nightly/cu121 --prerelease=allow  -
torch==2.3.0.dev20240219+cu121
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.3.0.dev20240219+cu121 and you require torch==2.3.0.dev20240219+cu121, we can conclude that the requirements are unsatisfiable.
```
I'm split 50/50 between _I'm doing something wrong_ vs. _This doesn't work correctly_



---

_Comment by @sebslight on 2024-02-20 05:14_

Related to #1451

---

_Closed by @sebslight on 2024-02-20 05:14_

---

_Referenced in [astral-sh/rye#668](../../astral-sh/rye/issues/668.md) on 2024-02-21 03:01_

---
