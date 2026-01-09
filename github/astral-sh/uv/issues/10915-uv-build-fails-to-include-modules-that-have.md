---
number: 10915
title: uv build fails to include modules that have symlinks elsewhere in project
type: issue
state: closed
author: tcjennings
labels:
  - question
  - compatibility
assignees: []
created_at: 2025-01-23T21:46:02Z
updated_at: 2025-01-25T22:58:41Z
url: https://github.com/astral-sh/uv/issues/10915
synced_at: 2026-01-07T13:12:18-06:00
---

# uv build fails to include modules that have symlinks elsewhere in project

---

_Issue opened by @tcjennings on 2025-01-23 21:46_

### Summary

As best as I can tell, `uv build` refuses to add modules to a wheel file when those modules have symlinks elsewhere in the project. These symlinks are not otherwise involved in the package tree. `hatch build` still works correctly.

### Minimal Reproducible Example
- uv 0.5.23

```
uv init --package youvee-test
cd youvee-test
mkdir -p src/youvee_test/foo/templates
mkdir -p src/youvee_test/foo/settings

touch src/youvee_test/__init__.py
touch src/youvee_test/foo/__init__.py
touch src/youvee_test/foo/settings/__init__.py
touch src/youvee_test/foo/templates/__init__.py
touch src/youvee_test/foo/templates/module.py
touch src/youvee_test/foo/templates/not_py.yaml
```

pyproject.toml
```
[project]
name = "youvee-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "...", email = "..." }
]
requires-python = ">=3.12"
dependencies = []

[project.scripts]
youvee-test = "youvee_test:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

`uv build` creates a correct wheel at this point:

```
❯ unzip -l dist/youvee_test-0.1.0-py3-none-any.whl
Archive:  dist/youvee_test-0.1.0-py3-none-any.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
       57  02-02-2020 00:00   youvee_test/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/settings/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/templates/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/templates/module.py
        0  02-02-2020 00:00   youvee_test/foo/templates/not_py.yaml
      166  02-02-2020 00:00   youvee_test-0.1.0.dist-info/METADATA
       87  02-02-2020 00:00   youvee_test-0.1.0.dist-info/WHEEL
       49  02-02-2020 00:00   youvee_test-0.1.0.dist-info/entry_points.txt
      834  02-02-2020 00:00   youvee_test-0.1.0.dist-info/RECORD
---------                     -------
     1193                     10 files
```

**Add a symlink farm to the project root.**
```
mkdir ./foo
pushd foo
ln -s ../src/youvee_test/foo/settings .
ln -s ../src/youvee_test/foo/templates .
popd
```

`uv build` creates an incorrect wheel now:
```
❯ unzip -l dist/youvee_test-0.1.0-py3-none-any.whl
Archive:  dist/youvee_test-0.1.0-py3-none-any.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
       57  02-02-2020 00:00   youvee_test/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/__init__.py
      166  02-02-2020 00:00   youvee_test-0.1.0.dist-info/METADATA
       87  02-02-2020 00:00   youvee_test-0.1.0.dist-info/WHEEL
       49  02-02-2020 00:00   youvee_test-0.1.0.dist-info/entry_points.txt
      475  02-02-2020 00:00   youvee_test-0.1.0.dist-info/RECORD
---------                     -------
      834                     6 files
```

`hatch build` creates a correct wheel:
```
❯ unzip -l dist/youvee_test-0.1.0-py3-none-any.whl
Archive:  dist/youvee_test-0.1.0-py3-none-any.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
       57  02-02-2020 00:00   youvee_test/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/settings/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/templates/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/templates/module.py
        0  02-02-2020 00:00   youvee_test/foo/templates/not_py.yaml
      166  02-02-2020 00:00   youvee_test-0.1.0.dist-info/METADATA
       87  02-02-2020 00:00   youvee_test-0.1.0.dist-info/WHEEL
       49  02-02-2020 00:00   youvee_test-0.1.0.dist-info/entry_points.txt
      836  02-02-2020 00:00   youvee_test-0.1.0.dist-info/RECORD
---------                     -------
     1195                     10 files
```

**Remove a symlink from the farm**
```
rm foo/settings
```

`uv build` is partially correct: the module that is not symlinked into the farm is now included:
```
❯ unzip -l dist/youvee_test-0.1.0-py3-none-any.whl
Archive:  dist/youvee_test-0.1.0-py3-none-any.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
       57  02-02-2020 00:00   youvee_test/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/__init__.py
        0  02-02-2020 00:00   youvee_test/foo/settings/__init__.py
      166  02-02-2020 00:00   youvee_test-0.1.0.dist-info/METADATA
       87  02-02-2020 00:00   youvee_test-0.1.0.dist-info/WHEEL
       49  02-02-2020 00:00   youvee_test-0.1.0.dist-info/entry_points.txt
      565  02-02-2020 00:00   youvee_test-0.1.0.dist-info/RECORD
---------                     -------
      924                     7 files
```


### Platform

Darwin 24.1.0 arm64

### Version

uv 0.5.23 (ba42467f1 2025-01-23)

### Python version

Python 3.13.0, Python 3.12.7

---

_Label `bug` added by @tcjennings on 2025-01-23 21:46_

---

_Comment by @charliermarsh on 2025-01-23 21:56_

Does `python -m build` (using https://github.com/pypa/build) produce the same result?

---

_Comment by @tcjennings on 2025-01-23 21:59_

For what it's worth, `python -m build` (without involving uv at all) has the same behavior.

```
rm -rf .venv
python -m venv .venv
activate .venv/bin/activate
pip install build
python -m build
```

---

_Comment by @zanieb on 2025-01-23 22:11_

If `python -m build` has the same behavior as `uv build` then this is a Hatch-specific problem, I think.

---

_Comment by @tcjennings on 2025-01-23 22:39_

I think I found the incantation that works around whatever the problem is: explicitly exclude and skip the symlink farm directory.

`pyproject.toml`:
```
[tool.hatch.build]
skip-excluded-dirs = true
exclude = ["/foo"]
```

---

_Label `bug` removed by @zanieb on 2025-01-23 22:52_

---

_Label `question` added by @zanieb on 2025-01-23 22:52_

---

_Label `compatibility` added by @zanieb on 2025-01-23 22:52_

---

_Closed by @charliermarsh on 2025-01-25 22:58_

---
