```yaml
number: 13128
title: ModuleNotFound when installing from local dependency
type: issue
state: closed
author: JuR-0
labels:
  - question
assignees: []
created_at: 2025-04-27T11:53:59Z
updated_at: 2025-04-27T13:44:14Z
url: https://github.com/astral-sh/uv/issues/13128
synced_at: 2026-01-12T16:01:20Z
```

# ModuleNotFound when installing from local dependency

---

_@JuR-0_

### Question

Hi there,

I have a ModuleNotFoundError arising on a local library.

My project has the following tree:

```
.
|-- common-lib
|   |-- pyproject.toml
|   |-- src
|   |   `-- common_lib
|   |       `-- __init__.py
|   |       
|   |-- tests
|   |   `-- __init__.py
|   `-- uv.lock
`-- other-lib
    |-- pyproject.toml
    |-- src
    |   `-- other_lib
    |       `-- __init__.py
    |-- tests
    |   `-- __init__.py
    `-- uv.lock
```

The pyproject.toml in common-lib is as follow:

```
[project]
name = "common-lib"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = []

[dependency-groups]
dev = [
    "pytest>=8.3.5",
]
test = [
    "pytest>=8.3.5",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build]
include = ["src"]
exclude = ["tests"]

[tool.pytest.ini_options]
pythonpath = 'src'
testpaths = ["tests"]
python_files = "test_*.py"
python_functions = "test_*"
```

And in other-lib:

```
[project]
name = "other-lib"
version = "0.1.0"

requires-python = ">=3.12"
dependencies = [
    "common_lib"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build]
include = ["src"]
exclude = ["tests"]

[dependency-groups]
dev = [
    "pytest>=8.3.5",
]
test = [
    "pytest>=8.3.5",
]


[tool.pytest.ini_options]
pythonpath = 'src'
testpaths = ["tests"]
python_files = "test_*.py"
python_functions = "test_*"

[tool.uv.sources]
common_lib = { path = "../common-lib", editable = true }
```

I have no problem running common-lib tests. But when I run other-lib tests I get the import error.
I have checked the following:
- common-lib is present in .venv folder in other-lib
- I have added a print of sys.path at the very beginning of other-lib tests and I can see the following:
`['/path_to_project/other-lib', '/path_to_project/other-lib/src', '/path_to_project/other-lib/.venv/bin', '/usr/local/lib/python312.zip', '/usr/local/lib/python3.12', '/usr/local/lib/python3.12/lib-dynload', '/path_to_project/other-lib/.venv/lib/python3.12/site-packages', '/path_to_project/common-lib', '/path_to_project/other-lib']`
- [Issue #10006](https://github.com/astral-sh/uv/issues/10006)

So the problem is that '/path_to_project/common-lib' is in python path but not '/path_to_project/common-lib/src' I suppose. How can I make this happen?

Thanks in advance for your time !

### Platform

mac OS 15

### Version

uv 0.5.29 (ca73c4754 2025-02-05)

---

_Label `question` added by @JuR-0 on 2025-04-27 11:53_

---

_Comment by @JuR-0 on 2025-04-27 13:44_

Adding 

```
[tool.hatch.build.targets.wheel]
packages = ["src/common_lib"]
```
In common-lib pyproject.toml fixed it


---

_Closed by @JuR-0 on 2025-04-27 13:44_

---
