```yaml
number: 7067
title: "\"Non workspaced\" monorepo editable dependencies path resolution is by relative path."
type: issue
state: closed
author: nrbnlulu
labels: []
assignees: []
created_at: 2024-09-05T05:37:14Z
updated_at: 2024-09-05T06:13:15Z
url: https://github.com/astral-sh/uv/issues/7067
synced_at: 2026-01-12T15:59:10Z
```

# "Non workspaced" monorepo editable dependencies path resolution is by relative path.

---

_@nrbnlulu_

Hey there, and thanks for uv!  
I am trying to setup a monorepo with editable dependencies.  
*(I don't want to use the workspace feature because
mostly because you can import stuff you are not allowed you don't need to (it also affects IDE completions)*
Anyways thats my setup:
```tree
.
├── hello.py
├── libs
│   └── sdk
│       ├── pyproject.toml
│       ├── README.md
│       ├── src
│       │   └── sdk
│       │       ├── amaizing.py
│       │       ├── __init__.py
│       └── uv.lock
├── LICENSE
├── pyproject.toml
├── README.md
└── uv.lock
```
libs/sdk/pyproject.toml
```toml
[project]
name = "sdk"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []


[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.metadata]
allow-direct-references = true

[tool.hatch.build.targets.wheel]
sources = ["src/sdk"]
```
./pyproject.toml
```toml
[project]
name = "uv-monorerpo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "sdk",
]

[tool.uv.sources]
sdk = { path = "libs/sdk", editable = true }
```
hello.py
```py
def main():
    def this_fails():
        from sdk.amaizing import so_amaizing  # what I would expect
        print(so_amaizing())
    def this_works():
        from libs.sdk.src.sdk.amaizing import so_amaizing # reality
        print(so_amaizing())
    this_works()
    this_fails()

if __name__ == "__main__":
    main()
```
As you can see you can only import the sdk package via its relative path from ./pyproject.toml (normalized with dots) which is kinda weird.

here is the demo project https://github.com/nrbnlulu/uv-monorerpo

---

_Renamed from ""Non workspaced" monorepo editable dependencies path resolution is by the relative path." to ""Non workspaced" monorepo editable dependencies path resolution is by relative path." by @nrbnlulu on 2024-09-05 05:48_

---

_Comment by @nrbnlulu on 2024-09-05 06:13_

Ok found it. you should remove this entry 
```toml
[tool.hatch.build.targets.wheel]
sources = ["src/sdk"]
```
otherwise it would generate wheels for whats inside "src/sdk" :disappointed: 

---

_Closed by @nrbnlulu on 2024-09-05 06:13_

---
