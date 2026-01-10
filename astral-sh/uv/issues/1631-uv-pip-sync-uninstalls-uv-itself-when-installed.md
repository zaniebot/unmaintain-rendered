---
number: 1631
title: "`uv pip sync` uninstalls uv itself when installed into the virtual env"
type: issue
state: closed
author: Tonius
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-02-18T07:57:28Z
updated_at: 2024-02-18T14:58:13Z
url: https://github.com/astral-sh/uv/issues/1631
synced_at: 2026-01-10T01:23:08Z
---

# `uv pip sync` uninstalls uv itself when installed into the virtual env

---

_Issue opened by @Tonius on 2024-02-18 07:57_

## Issue description

When creating a new virtual env and installing `uv` into it, running `uv pip sync` will cause it to uninstall itself.

This is inconsistent with pip-tools, whose sync command explicitly ignores pip-tools itself, as described here: https://github.com/jazzband/pip-tools?tab=readme-ov-file#example-usage-for-pip-sync

> Note: pip-sync will not upgrade or uninstall packaging tools like setuptools, pip, or pip-tools itself.

## Example

```
$ python3.11 -m venv .env

$ . .env/bin/activate

(.env) $ pip install uv

  Collecting uv
    Obtaining dependency information for uv from https://files.pythonhosted.org/packages/d4/2e/4c60d328ffdd0e2dc824ebec08117c8034e3a505b4f58ff900f1c1519791/uv-0.1.4-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
    Downloading uv-0.1.4-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (17 kB)
  Downloading uv-0.1.4-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (10.1 MB)
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 10.1/10.1 MB 13.6 MB/s eta 0:00:00
  Installing collected packages: uv
  Successfully installed uv-0.1.4

  [notice] A new release of pip is available: 23.2.1 -> 24.0
  [notice] To update, run: pip install --upgrade pip

(.env) $ echo "ruff==0.2.2" > requirements.in

(.env) $ python -m uv pip compile requirements.in --output-file=requirements.txt
  
  Resolved 1 package in 547ms

(.env) $ python -m uv pip sync requirements.txt

  Resolved 1 package in 28ms
  Downloaded 1 package in 465ms
  Uninstalled 1 package in 1ms
  Installed 1 package in 8ms
   + ruff==0.2.2
   - uv==0.1.4

(.env) $ python -m uv pip sync requirements.txt

  .env/bin/python: No module named uv
```

---

_Label `bug` added by @zanieb on 2024-02-18 08:13_

---

_Comment by @zanieb on 2024-02-18 08:13_

Yeah we should probably avoid this :)

---

_Label `good first issue` added by @zanieb on 2024-02-18 08:13_

---

_Referenced in [astral-sh/uv#1649](../../astral-sh/uv/pulls/1649.md) on 2024-02-18 14:35_

---

_Closed by @charliermarsh on 2024-02-18 14:39_

---

_Comment by @jasonwashburn on 2024-02-18 14:58_

Ha...seemed like a good project for the morning...come back to submit PR and it's already fixed/merged. Even the fixes come surprisingly fast. 👍 

---
