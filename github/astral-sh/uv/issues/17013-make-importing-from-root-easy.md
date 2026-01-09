---
number: 17013
title: Make importing from root easy
type: issue
state: open
author: davidgilbertson
labels:
  - enhancement
assignees: []
created_at: 2025-12-06T05:14:46Z
updated_at: 2025-12-16T20:59:00Z
url: https://github.com/astral-sh/uv/issues/17013
synced_at: 2026-01-07T13:12:19-06:00
---

# Make importing from root easy

---

_Issue opened by @davidgilbertson on 2025-12-06 05:14_

### Summary

A common use case is to have an 'application' type project (not a library), and to allow importing of shared code, like `from utils import my_util`, where `utils` is a directory at the top level of the project.

## Background - the current pain

This is _possible_ right now, but for such a common requirement you really need to dig quite deep to work out how to get it working. It took me a while to work out that the minimum I needed was a file `.venv/Lib/site-packages/my_project.pth` that contained my root directory path, which is added to `sys.path`.
And that any top-level directory that I want to import (`utils`, `common`, etc) should have an `__init__.py` file in it.

To get this working, my `pyconfig.toml` looks like this:
```toml
[build-system]
requires = ["uv_build>=0.9.15,<0.10.0"]
build-backend = "uv_build"

[tool.uv.build-backend]
module-root = ""
module-name = []
```
I'm not even sure that `module-name = []` is valid, but it works. I can make that `module-name = ["utils", "common"]` but it doesn't change the `.pth` file and it's not DRY. 
Part of what makes this difficult is that when you get into the docs of any of the build tools it all assumes you're distributing a wheel so nothing really clicks and you get this feeling that you're doing things the wrong way.

## Suggested solution

So my feature request is to support a property in `pyproject.toml` that does the job in one line. Like `enable_imports = true`. And when I `uv sync` such a project, it says "ah, this user wants to do top-level imports, we'll create a `.pth` file with the project root for them, they'll be so happy!".
Or maybe even take the view that the _default_ behaviour for an application should be to add that `.pth` file, the downside is pretty low (a user has an import that would otherwise fail, but succeeds because it matches a directory in the project root with an `__init__.py` file in it).

### Example

_No response_

---

_Label `enhancement` added by @davidgilbertson on 2025-12-06 05:14_

---

_Comment by @konstin on 2025-12-16 11:47_

Can you share for project structure you have? I tried reproducing it but it works for me: https://github.com/konstin/uv-17013-top-level-utils

---

_Comment by @davidgilbertson on 2025-12-16 20:05_

Importing from root will work fine from a file that's also at the top level, but try importing from utils or common from within a subdirectory. 
Also you won't be able replicate in PyCharm Python Console which helps by adding the root for to sys.path

---

_Comment by @davidgilbertson on 2025-12-16 20:59_

OK I checked out your repo. I'm not sure what you're saying, what is it that you're trying to replicate exactly?

Some points to clarify:
 - Yes, by configuring `[build-system]` and `[tool.uv.build-backend]` with just the right values in `pyproject.toml`, you will be able to import from top-level directories.
 - Your config has `module-name = ["top_level_utils", "common", "utils"]` which works, but means every time you add a new top-level directory you also need to add it to the config and re-build the project. Duplication of work and not ideal.
 - Even without this, in your repo if you imported from within `main.py` that would work because it's at the top level.
 - Without the config of build system and backend settings, importing from `utils` or `common` (from anywhere but a file at the project root) would fail.

So the thing to replicate is that there's no way to allow imports from top-level directories that doesn't require several lines of config that probably most people would need to search the docs for each time they want to do it.

---
