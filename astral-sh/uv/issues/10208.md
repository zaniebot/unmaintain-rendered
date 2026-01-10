```yaml
number: 10208
title: Shadow projects
type: issue
state: closed
author: T-256
labels:
  - needs-design
assignees: []
created_at: 2024-12-27T21:59:56Z
updated_at: 2025-06-17T00:05:08Z
url: https://github.com/astral-sh/uv/issues/10208
synced_at: 2026-01-10T03:32:45Z
```

# Shadow projects

---

_Issue opened by @T-256 on 2024-12-27 21:59_

Opened this to discuss and get comments about implementing new kind of projects rather than standard `project` defined in `pyproject.toml`.

A Shadow project benefits all of `tool.uv` features, in addition to standard feathers like `dependency-groups`. its main purpose is to make and keep sync environments with declared dependencies while keeps dependency features from `project` section.

- Disallow to build or publish it since it isn't package (not lib nor app).
  - Conflicts with `tool.uv.package = true`.
  - Conflicts with `build-system`.
  - Not have any entry points like `hello.py` file or `src/example` folder.
- SCM disabled by default
- Despite `project` table, the `tool.uv.shadow` table only accept: `requires-python`, `dependencies` and `optional-dependencies`.
  - For uv's output logs, could use `shadow` constant instead of `project.name` and `0.1.0` constant instead of `project.version`.
  - Conflicts with `project`.
  - No `Readme.md` file.
- It couldn't be a workspace member and conflicts with `tool.uv.workspace` definition too. 


### Example
`Initializing`
```sh
$ uv init demo --shadow
Initialized shadow at `./demo`
```

`demo/pyproject.toml`
```toml
[tool.uv.shadow]
requires-python = ">=3.12"
dependencies = [
    "mkdocs>=1.6.1",
]
```
`uv sync`
```sh
Using CPython 3.12.4 interpreter at: path/to/python.exe
Creating virtual environment at: .venv
Resolved 22 packages in 1.02s
Prepared 5 packages in 2.19s
Installed 19 packages in 144ms
 + click==8.1.8
 + colorama==0.4.6
 + ghp-import==2.1.0
 + importlib-metadata==8.5.0
 + jinja2==3.1.5
 + markdown==3.7
 + markupsafe==2.1.5
 + mergedeep==1.3.4
 + mkdocs==1.6.1
 + mkdocs-get-deps==0.2.0
 + packaging==24.2
 + pathspec==0.12.1
 + platformdirs==4.3.6
 + python-dateutil==2.9.0.post0
 + pyyaml==6.0.2
 + pyyaml-env-tag==0.1
 + six==1.17.0
 + watchdog==4.0.2
 + zipp==3.20.2
```
`final files structure`
```
.
│ .venv
│ .python-version
│ pyproject.toml
│ uv.lock
```

---

_Label `needs-design` added by @charliermarsh on 2024-12-28 14:38_

---

_Comment by @T-256 on 2025-06-17 00:05_

See https://github.com/astral-sh/uv/issues/10204#issuecomment-2978496593

---

_Closed by @T-256 on 2025-06-17 00:05_

---
