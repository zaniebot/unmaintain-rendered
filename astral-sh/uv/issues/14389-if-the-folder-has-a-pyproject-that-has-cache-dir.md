```yaml
number: 14389
title: "If the folder has a pyproject that has `cache-dir=\".venv/cache\"` uv venv always fails because the dir already exists"
type: issue
state: closed
author: WilliamStam
labels:
  - question
assignees: []
created_at: 2025-07-01T10:55:48Z
updated_at: 2025-07-03T14:25:01Z
url: https://github.com/astral-sh/uv/issues/14389
synced_at: 2026-01-12T16:01:47Z
```

# If the folder has a pyproject that has `cache-dir=".venv/cache"` uv venv always fails because the dir already exists

---

_@WilliamStam_

### Summary

pyproject.toml
```
[project]
name = "testing"

[tool.uv]
cache-dir=".venv/cache"
```

have uv installed globally. when running uv we get this

```
W:\Temp\aa>uv venv
Using CPython 3.13.3 interpreter at: C:\Python\Python313\python.exe
Creating virtual environment at: .venv
uv::venv::creation

  x Failed to create virtualenv
  `-> The directory `.venv` exists, but it's not a virtual environment

W:\Temp\aa>
```

looks like uv creates the folder for cache first then fails later on. 

this is very low priority but its had me scratching my head for a few weeks trying to figure out why i can never seem to use uv to create a project (copy paste pyproject.toml and edit the project stuff mostly)

### Platform

windows 11

### Version

uv 0.7.17 (41c218a89 2025-06-29)

### Python version

Python 3.13.3

---

_Label `bug` added by @WilliamStam on 2025-07-01 10:55_

---

_Comment by @jtfmumm on 2025-07-01 12:35_

I can see how this could be surprising. For now, you can use `uv venv --allow-existing` for this case.

---

_Comment by @zanieb on 2025-07-01 12:56_

We wouldn't recommend putting the cache in the virtual environment

---

_Label `bug` removed by @konstin on 2025-07-01 13:59_

---

_Label `question` added by @konstin on 2025-07-01 13:59_

---

_Closed by @charliermarsh on 2025-07-03 13:15_

---

_Comment by @WilliamStam on 2025-07-03 13:34_

yeah but no where does it actually say that (dont put the cache dir in the venv folder). and it comes as a "surprise" as was pointed out. as a developer surprises are bad. 

placing the cache in the venv isnt the worst idea ever if your filesystem is heavily restricted and you only have access to the project folder. alternatively you could pollute the root folder with a .cache folder or some such but that is purely meant for uv. so it makes sense to self contain as much as possible. 

whilst most of the time needing to set the cache folder is not needed, for the few cases where it is (to such a degree that there is even an option for it) then it shouldn't "break" other functionality when used like this. 

---

_Comment by @zanieb on 2025-07-03 14:25_

I'm sorry it caused you a problem and is surprising, but by putting it in the `.venv` directory you include _persistent_ content in a directory that is intended to be disposable. We can't recreate a virtual environment with content from the cache if the cache is inside the virtual environment. Since the content you're adding to the virtual environment isn't a part of the expected structure for a virtual environment, we refuse to use the directory for safety reasons.

---
