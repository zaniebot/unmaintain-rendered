---
number: 14464
title: No way to add existing projects to a workspace
type: issue
state: closed
author: kyle-basis
labels:
  - enhancement
  - good first issue
  - help wanted
assignees: []
created_at: 2025-07-05T03:38:34Z
updated_at: 2025-07-09T15:46:55Z
url: https://github.com/astral-sh/uv/issues/14464
synced_at: 2026-01-10T01:25:45Z
---

# No way to add existing projects to a workspace

---

_Issue opened by @kyle-basis on 2025-07-05 03:38_

### Summary

Hi, I'm trying to scriptify `uv` to create a workspace automatically for a CMake tree (foolish, I know). I have something working via creating a `pyproject.toml` by hand, but I'd love to use `uv` to script this instead. Unfortunately there doesn't seem to be a great way to add existing projects to a workspace. My choices are `uv add <project>` which doesn't add to `tool.uv.workspace` (nor flag the member as workspace), or `uv init` which doesn't have any flag such as `--exists`. #5388 exists, but doesn't quite cover what I need.

The end result I want is something like this:
```
[project]
name = "basis_cmake"
requires-python = ">=3.12"
version = "0.0.0"
dependencies = [
  "basis-unit-pybind-test",
]

[build-system]
requires = [
  "setuptools",
]
build-backend = "setuptools.build_meta"

[tool.setuptools]
py-modules = []

[tool.uv.sources]
basis-unit-pybind-test = { workspace = true }
[tool.uv.workspace]
members = [
  "/basis/unit/pybind_test/py",
]

[dependency-groups]
dev = [
    "jinja2>=3.1.6",
    "jsonschema>=4.24.0",
    "pyyaml>=6.0.2",
]
```

This is almost scriptable, just no way to add to workspace, I have a nearly working example that looks like:
```
        execute_process(COMMAND ${UV} init
                            --name ${UV_PROJECT_NAME}
                            --bare
                            --no-readme
                            --no-description
                            --lib
                            --author-from none
                            --python ${UV_PYTHON_VERSION}
                            --build-backend uv
                        COMMAND_ERROR_IS_FATAL ANY)
        execute_process(COMMAND ${UV} version ${UV_PROJECT_VERSION}
                        COMMAND_ERROR_IS_FATAL ANY)
```
but I'm forced to instead manually write toml line by line.

If there's support from the `uv` team I'd be happy to "de-rust" my rust and contribute.

### Example

Either:
`uv add --workspace <path>` could add a project to the workspace with the proper pathing
or:
`uv init --existing --bare` (or similar) could add an existing project to a workspace without touching anything else


---

_Label `enhancement` added by @kyle-basis on 2025-07-05 03:38_

---

_Comment by @charliermarsh on 2025-07-07 14:42_

Yeah I think `--workspace` should be supported.

---

_Label `help wanted` added by @charliermarsh on 2025-07-07 14:42_

---

_Label `good first issue` added by @zanieb on 2025-07-07 16:41_

---

_Referenced in [astral-sh/uv#14496](../../astral-sh/uv/pulls/14496.md) on 2025-07-07 20:56_

---

_Comment by @kyle-basis on 2025-07-08 03:27_

Dangit @charliermarsh  - while I appreciate the quick resolution I was kind of excited to have a chance to contribute. :)

---

_Closed by @charliermarsh on 2025-07-09 15:46_

---
