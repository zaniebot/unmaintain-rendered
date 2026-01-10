---
number: 8750
title: Editable install for dependencies without pyproject.toml or setup.py
type: issue
state: open
author: mpasson
labels:
  - question
assignees: []
created_at: 2024-11-01T11:50:28Z
updated_at: 2024-11-01T14:16:23Z
url: https://github.com/astral-sh/uv/issues/8750
synced_at: 2026-01-10T01:24:32Z
---

# Editable install for dependencies without pyproject.toml or setup.py

---

_Issue opened by @mpasson on 2024-11-01 11:50_

I have a few questions about the 'editable' option for dependencies pointing at local path. In order to test it, I set up the following toy model:

```bash
.
├── dependency1
│   ├── README.md
│   ├── dependency1
│   │   ├── __init__.py
│   │   └── _version.py
│   ├── pyproject.toml
│   └── uv.lock
└── package1
    ├── README.md
    ├── hello.py
    ├── package1
    │   ├── __init__.py
    │   └── _version.py
    ├── pyproject.toml
    └── uv.lock
```

The idea is to have `dependency1` as a local editable dependency for `package1` . In order to do that, I run 

```bash
uv add  <full-path-to-dependency1> --editable
```

which correctly add the source to `project1` *pyproject.toml* as:

```toml
[tool.uv.sources]
dependency1 = { path = "../dependency1", editable = true }
```

My questions are the following:
1. Is there a way to keep the full path of `dependency1` in the *pyptoject.toml* of `package1`, instead of resolving to the relative one? This would make `project1` more transferable.
2. All this works provided `dependency1` is an installable package, and it seem the build process is triggered during creation of the uv environment. It would be possible to avoid this step and just link `dependency1` in the uv environment (maybe with a .pth file)? This would avoid messing up with the local `dependency1` folder (right now build hooks, such as hatch vcs, are triggered) and allow the installation of dependencies without a *pyproject.toml*.




---

_Comment by @charliermarsh on 2024-11-01 13:26_

(1) We won't write the absolute PATH, but you can replace `path = "../dependency"` with the absolute path by editing the `pyproject.toml` yourself. It makes your project a lot less portable, e.g., you can't check-in your `pyproject.toml` to Git and share it with others, since you'll have an absolute path that's absolute to your machine or environment.

---

_Comment by @charliermarsh on 2024-11-01 13:28_

(2) No, we have to run the build process in order to create a `.pth` file. You can mark `dependency1` as a non-package by setting `package = false` under `[tool.uv]` (or removing the `[build-system]` table). That would lead us to install the dependencies of `dependency1`, but not the package itself. Which you may or may not want? You wouldn't be able to import from `dependency1`. (To install as editable, we have to run the build backend hooks.)


---

_Label `question` added by @charliermarsh on 2024-11-01 13:28_

---

_Comment by @mpasson on 2024-11-01 14:11_

Thanks for the quick answers! A few comments:
(1) Yes that solves it (agree, to use with caution). 
(2) I guess what I'm looking for is for a way to add specific local folders in the search path for the virtual environment created by uv, and for it to happen automatically when the environment is build or updated (including eventual removal). The local folders contains python modules that should be imported in the environment, but are provided "as is" and are not properly packaged python modules (no pyproject.toml). How would you approach this?

---

_Comment by @charliermarsh on 2024-11-01 14:16_

I don't have much to offer on (2) unfortunately. If you're trying to make the project installable, it should really be structured as a package. I'd suggest making them "real" projects and using a build backend like hatchling. When they're installed, they'll be installed as editables (`.pth` files), so they'll automatically update on code change etc.


---
