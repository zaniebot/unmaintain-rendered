```yaml
number: 6445
title: install cli scripts into the virtual environment
type: issue
state: closed
author: natelandau
labels:
  - question
assignees: []
created_at: 2024-08-22T16:11:05Z
updated_at: 2024-08-22T22:30:59Z
url: https://github.com/astral-sh/uv/issues/6445
synced_at: 2026-01-12T15:59:04Z
```

# install cli scripts into the virtual environment

---

_@natelandau_

Platform and UV information: `uv 0.3.1 (Homebrew 2024-08-21)` running on `MacOS Sonoma 14.1.2`

I am working to migrate a number of packages from Poetry to uv.  Perhaps I'm just not understanding the documentation, but I can't figure out how to install CLIs into the venv created by uv. 

I manage multiple CLIs out of single packages. During development, I need to run the CLIs within the virtual environment. How would this be accomplished using uv?

Minimal package layout:
```
├── pyproject.toml
├── src/
    └── package_name/
        ├── __init__.py
        ├── cli1.py
        ├── cli2.py
        ├── models/
        ├── utils/
        ├── ...
```

This was easily accomplished in Poetry using `tool.poetry.scripts` in the `pyproject.toml` file. This would allow invoking any of the cli scripts within the virtual environment rather than the running the globally installed version

```toml
[tool.poetry.scripts]
    cli1 = "package_name.cli1:app"
    cli2 = "package_name.cli2:app"
```

is similar functionality available in uv?  or a workaround that I'm not seeing?  

---

_Renamed from "migration from poetry: `tool.poetry.scripts` " to "install cli scripts into the virtual environment" by @natelandau on 2024-08-22 16:11_

---

_Comment by @zanieb on 2024-08-22 16:17_

Hey! We support the standard [`[project.scripts]`](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#creating-executable-scripts) table instead of declaring some tool-specific section :)

---

_Comment by @zanieb on 2024-08-22 16:17_

For my own reference, mention this in a Poetry migration guide #5200

---

_Label `question` added by @zanieb on 2024-08-22 16:17_

---

_Closed by @natelandau on 2024-08-22 22:30_

---
