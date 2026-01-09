---
number: 13138
title: "`uv pip install --group` fails where `pip install --group` succeeds"
type: issue
state: closed
author: leodevian
labels:
  - bug
assignees: []
created_at: 2025-04-27T20:00:35Z
updated_at: 2025-06-13T22:16:50Z
url: https://github.com/astral-sh/uv/issues/13138
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip install --group` fails where `pip install --group` succeeds

---

_Issue opened by @leodevian on 2025-04-27 20:00_

### Summary

Hello,

It seems that `uv pip install --group` and `pip install --group` (pip 25.1) have different behaviors for installing Dependency Groups.

```toml
[dependency-groups]
dev = [
  "ruff",
]
```

```console
$ uv pip install --group dev
warning: `.\scripts` does not appear to be a Python project, as the `pyproject.toml` does not include a `[build-system]` table, and neither `setup.py` nor `setup.cfg` are present in the directory
error: The dependency group 'dev' was not found in the project: pyproject.toml
```

```console
$ pip install --group dev
Collecting ruff
  Using cached ruff-0.11.7-py3-none-win_amd64.whl.metadata (26 kB)
Using cached ruff-0.11.7-py3-none-win_amd64.whl (11.6 MB)
Installing collected packages: ruff
Successfully installed ruff-0.11.7
```

As PEP 735 specifies that [Dependency Groups support non-package projects and that the installation of a Dependency Group does not imply installation of a package's dependencies or the package itself](https://peps.python.org/pep-0735/#dependency-groups-are-not-hidden-extras), I believe that the behavior of pip is the expected behavior.

Thank you 🙏 

### Platform

Windows 11 amd64

### Version

uv 0.6.17 (8414e9f3d 2025-04-25)

### Python version

Python 3.13.2

---

_Label `bug` added by @leodevian on 2025-04-27 20:00_

---

_Renamed from "`uv pip install --group` fails if the `[project]` table is missing" to "`uv pip install --group` fails where `pip install --group` succeeds" by @leodevian on 2025-04-27 20:00_

---

_Comment by @leodevian on 2025-04-27 20:09_

It seems that the `uv pip compile` command cannot be used to lock dependencies if the `[project]` table is missing.

---

_Assigned to @Gankra by @charliermarsh on 2025-04-28 00:42_

---

_Comment by @charliermarsh on 2025-04-28 00:42_

\cc @Gankra -- we need to allow dependency groups in `pyproject.toml` files that are not valid Python packages.

---

_Comment by @alexprengere on 2025-05-13 09:36_

I just want to mention that this will occur also if the `pyproject.toml` has a `[build-system]`, but the rest of the packaging is specified in `setup.cfg` and/or `setup.py`, which I think is pretty common (as not every setuptools-based packages have fully migrated to `pyproject.toml`).
As @leodevian mentions, the trigger is when `[project]` table is missing.

---

_Referenced in [astral-sh/uv#13742](../../astral-sh/uv/pulls/13742.md) on 2025-05-30 20:02_

---

_Closed by @Gankra on 2025-06-13 22:16_

---
