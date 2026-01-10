---
number: 7576
title: UV Workspaces setup  and imports
type: issue
state: closed
author: pablo-a
labels: []
assignees: []
created_at: 2024-09-20T10:00:53Z
updated_at: 2025-03-20T15:38:22Z
url: https://github.com/astral-sh/uv/issues/7576
synced_at: 2026-01-10T01:24:17Z
---

# UV Workspaces setup  and imports

---

_Issue opened by @pablo-a on 2024-09-20 10:00_

Hi ! Thanks a lot for this really promising project !
I however fail to understand how to setup correctly uv workspaces even for a minimalist project that follow uv documentation

TLDR;
i have a repository containing an app and a package. I fail to import the package code in the app code
here is a link to the public repository with full code i made to try out uv workspaces : https://github.com/pablo-a/uv_workspaces/tree/master

## Repository structure 
```
.
└── uv_workspaces ## Repository name/
    ├── pyproject.toml
    ├── .python-version
    ├── uv.lock
    ├── packages/
    │   └── p1/
    │       ├── hello.py
    │       ├── pyproject.toml
    │       └── .python-version
    └── src/
        └── uv_workspaces/
            └── hello.py
```

## app hello.py
```
from p1 import main as main_p1

def main():
    main_p1()
    print("Hello from uv-workspaces!")

if __name__ == "__main__":
    main()
```

## Command run and error :

```
uv run src/uv_workspaces/hello.py
Traceback (most recent call last):
  File "/Users/pabloabril/Documents/deepki/uv_workspaces/src/uv_workspaces/hello.py", line 1, in <module>
    from p1.hello import main as main_p1
ModuleNotFoundError: No module named 'p1'
```

uv version : uv 0.4.10 (690716484 2024-09-13)
platform: Macos M1 pro ARM64



## Same error for example repo uv-fastapi-example
It seems to me that i'm not running the good command ? for example i tried cloning your uv-fastapi-example repository and ran into the same error :
```
uv run app/main.py
Traceback (most recent call last):
  File "/Users/pabloabril/Documents/deepki/uv-fastapi-example/app/main.py", line 3, in <module>
    from .dependencies import get_query_token, get_token_header
ImportError: attempted relative import with no known parent package
```

Thanks a lot for your hard work and let me know if i can give any more information to help.

---

_Comment by @BaccanoMob on 2024-09-21 04:54_

Use this folder structure (for some reason p1 did'nt work but pone did).

Packages have a different `folder structure` and different `pyproject.toml` compared to app ([Check docs](https://docs.astral.sh/uv/concepts/projects/)) which packages folder did not follow.

```
.
├── package/
│   └── pone/
│       ├── src/
│       │   └── pone/
│       │       ├── __init__.py
│       │       └── hello.py
│       ├── .python-version
│       ├── pyproject.toml
│       └── README.md
├── src/
│   └── uv_workspaces/
│       ├── __init__.py
│       └── hello.py
├── .python-version
├── pyproject.toml
└── README.md
```
pone/pyproject.toml
```toml
[project]
name = "pone"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = []

# BELOW ARE REQUIRED FOR PACKAGES
[tool.hatch.build.targets.wheel]
packages = ["src/pone"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

pyproject.toml
```toml
[project]
name = "uv-workspaces"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "pone",
]

[tool.uv.workspace]
members = ["packages/pone"]

[tool.uv.sources]
pone = { workspace = true }

# BELOW ARE REQUIRED FOR PACKAGED APPS
[tool.hatch.build.targets.wheel]
packages = ["src/uv_workspaces"]

[project.scripts]
hello = "uv_workspaces.hello:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Add `from pone.hello import main as main_p1` in hello.py

run this with

```
uv run hello
```
Do not run packaged app with `uv run src/uv_workspaces/hello.py`, use project.scripts. For your case, since its one file it would still work but with larger projects you might get import errors when using relative imports (thats how i found this issue). 

pone package can also have project.scripts and can run with `uv run` (for below it would be `uv run hello_pone`) so you can test pone package independently.

```toml
[project.scripts]
hello_pone = "pone.hello:main"
```

Delete .venv, uv.lock, __pycache__ and try again if the above changes do not work.

---

_Comment by @pablo-a on 2024-09-22 10:40_

Thanks a lot ! I understand it a lot better now,
i was missing the part about including the build-system for each package that can be importable :)

I still find it a bit confusing, not being able to run a file directly without invoking it from pyproject.script section.
Feel like workspaces section could use some clarifications :)

Thanks a lot anyway !



---

_Closed by @pablo-a on 2024-09-22 10:40_

---

_Comment by @BaccanoMob on 2024-09-22 13:10_

> I still find it a bit confusing, not being able to run a file directly without invoking it from pyproject.script section.

I think it will run if the hello.py was in the root directory rather than inside src/uv_workspaces folder. 

If uses case is package or packaged app -> it needs to be under src folder (or you will need to specify a path in tool.hatch.build.targets.wheel), this is related to building the package

If its app -> root folder, does not need src folder

Check all the project structure in the [docs](https://docs.astral.sh/uv/concepts/projects/) where the hello function is and how it is called for different projects

For bigger applications like the fastapi example, you prolly need to move main.py (aka entrypoint) to root directory (and remove relative imports in the files in root directory) for it to work as an application. This is more of a python related issue rather than uv why it needs to be in the root directory. (I do not know python enough to resolve this sorry)

---

_Comment by @C-Loftus on 2025-03-20 14:42_

Sorry to bump an old thread but do others find that you can go to type definition or use similar static tools when using uv workspaces? In vscode my common package can be imported from, gets syntax highlighting, and works fine at runtime, but there appears to be something wrong with module resolution since I can't jump to definitions. 

![Image](https://github.com/user-attachments/assets/21378197-6112-40d1-9a41-bb35c9c8f85b)

---

_Comment by @C-Loftus on 2025-03-20 15:11_

> Sorry to bump an old thread but do others find that you can go to type definition or use similar static tools when using uv workspaces? 

Figured this out sorry to bump again! In my case it was a lib so turns out you need to run `uv init --lib $YOUR_PACKAGE_DIR/$YOUR_PACKAGE_NAME`. Not exactly sure why this works and the cli doesn't but notably the lib puts a `py.typed` file in the directory which might be relevant? It also generates the package with a `src/` directory which I didn't have before.

tldr use the cli options I guess


---

_Comment by @alexcochran on 2025-03-20 15:11_

> Sorry to bump an old thread but do others find that you can go to type definition or use similar static tools when using uv workspaces? In vscode my common package can be imported from, gets syntax highlighting, and works fine at runtime, but there appears to be something wrong with module resolution since I can't jump to definitions.
> 
> ![Image](https://github.com/user-attachments/assets/21378197-6112-40d1-9a41-bb35c9c8f85b)

I'm noticing something similar in my VS Code environment while using [someone else's working example](https://github.com/fedragon/uv-workspace-example/tree/main#). I can successfully run a script in the root-level package that imports and executes a function from the subpackage, but Pylance complains that the package import can't be resolved, and I can't navigate to the definitions:

![Image](https://github.com/user-attachments/assets/7610af46-4bb2-4ff7-b9f5-8ff7328a9b4a)

---

_Comment by @C-Loftus on 2025-03-20 15:38_

> I'm noticing something similar in my VS Code environment while using [someone else's working example](https://github.com/fedragon/uv-workspace-example/tree/main#). I can successfully run a script in the root-level package that imports and executes a function from the subpackage, but Pylance complains that the package import can't be resolved, and I can't navigate to the definitions:

@alexcochran Not sure about your specific example, but if it helps I have now have a working project with uv workspaces, 2 packages, and one library; it works with vscode and pylance for me. It is all open source so feel free to take a look https://github.com/cgs-earth/IODH-Mappings It should be straightforward to build with the makefile at the root.

---

_Referenced in [astral-sh/uv#9637](../../astral-sh/uv/issues/9637.md) on 2025-05-22 02:46_

---

_Referenced in [thromer/uv-monorepo-example#1](../../thromer/uv-monorepo-example/pulls/1.md) on 2025-06-29 21:52_

---
