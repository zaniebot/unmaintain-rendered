---
number: 12395
title: "`uv init` Add a .gitignore file?"
type: issue
state: closed
author: jordan-carson
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-03-22T18:48:12Z
updated_at: 2025-08-16T12:36:41Z
url: https://github.com/astral-sh/uv/issues/12395
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv init` Add a .gitignore file?

---

_Issue opened by @jordan-carson on 2025-03-22 18:48_

### Question

What is the community's thoughts on adding a basic `.gitignore` file during `uv init`? My rationale is given `uv init` creates a `.venv` folder, which should never be pushed to a git, it would be helpful to add an ignore file to include the `.venv`.

Simple ignore file such as 

```
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# Environments
.env
.venv

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg


# Unit test / coverage reports
htmlcov/
.tox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
.hypothesis/
.pytest_cache/

# IDE specific files
.idea/
.vscode/
*.swp
*.swo
```

### Platform

_No response_

### Version

0.5.2

---

_Label `question` added by @jordan-carson on 2025-03-22 18:48_

---

_Comment by @nathanjmcdougall on 2025-03-23 09:04_

It's worth noting that the `.venv` directory itself contains a `.gitignore` file which prevents it being pushed to git.

---

_Comment by @jordan-carson on 2025-03-23 15:15_

> It's worth noting that the `.venv` directory itself contains a `.gitignore` file which prevents it being pushed to git.

aye yes it does! the good ole exclude everything `*`. 

---

_Referenced in [astral-sh/uv#12542](../../astral-sh/uv/issues/12542.md) on 2025-03-29 07:33_

---

_Label `question` removed by @zanieb on 2025-04-01 17:35_

---

_Label `enhancement` added by @zanieb on 2025-04-01 17:35_

---

_Label `needs-decision` added by @zanieb on 2025-04-01 17:35_

---

_Comment by @zanieb on 2025-04-01 17:36_

It'd be sensible to add something with `.pyc` ignores and such. I see it as a low priority though.

---

_Comment by @nathanjmcdougall on 2025-04-15 03:01_

As per #12886 I think this is already a feature - in my test `uv init` creates a `.gitignore` file with these contents:

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

---

_Comment by @WangYihang on 2025-04-22 12:49_

Please use https://github.com/github/gitignore/blob/main/Python.gitignore.

---

_Comment by @konstin on 2025-04-22 16:00_

Note that uv removes the need for some of these entries:
* `dist`: uv adds a `.gitignore` to the `dist` folder it creates.
* `build/` and `*.egg-info`: Unless you are using setuptools, these are not created anymore.
* `.venv`:  uv adds a `.gitignore` to each venv it creates.

---

_Comment by @DipakKhade on 2025-04-24 11:23_

it seems like .gitignore is added in uv init 

<img width="634" alt="Image" src="https://github.com/user-attachments/assets/12e39a6a-a187-44ec-b26c-b7e2041df8b8" />
can close this issue

---

_Comment by @charliermarsh on 2025-05-04 12:48_

Personally, I'd say we should use `Python.gitignore`, since it's already curated and the closest thing I can think of to an "industry standard".


---

_Comment by @wabisabia on 2025-07-01 11:22_

On a related note: it may be helpful to avoid creating a `.gitignore` file in the context of a workspace where one likely already exists.

Introducing a `--no-gitignore` flag would provide flexibility even beyond the workspace use-case. I could then see `--no-gitignore` later being inferred for workspaces.

---

_Comment by @mark-P0 on 2025-08-16 05:53_

Seems like the `.gitignore` file is added in as part of the `git` repo setup. If there's already a repo, no `.gitignore` file is created.

---

_Comment by @injust on 2025-08-16 10:22_

https://github.com/github/gitignore/blob/main/Python.gitignore is pretty comprehensive, which is great when I actively include it myself and know what's inside. But as a hands-off, just-drop-it-in default, I wonder if it's too much.

For example, it ignores `.envrc`. If you want to [use direnv with uv](https://github.com/direnv/direnv/wiki/Python#uv), you're meant to manually comment out that line in the gitignore.

---

_Comment by @zanieb on 2025-08-16 12:36_

```
❯ uv init example
Initialized project `example` at `/private/tmp/example`
❯ cat example/.gitignore
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

I think we should close this?

---

_Closed by @zanieb on 2025-08-16 12:36_

---
