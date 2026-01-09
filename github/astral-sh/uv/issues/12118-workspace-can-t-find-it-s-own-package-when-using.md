---
number: 12118
title: "workspace can't find it's own package when using __main__.py"
type: issue
state: closed
author: andersfylling
labels:
  - question
assignees: []
created_at: 2025-03-11T16:46:03Z
updated_at: 2025-03-13T08:54:49Z
url: https://github.com/astral-sh/uv/issues/12118
synced_at: 2026-01-07T13:12:18-06:00
---

# workspace can't find it's own package when using __main__.py

---

_Issue opened by @andersfylling on 2025-03-11 16:46_

### Question

Hello, I'm sure this is a misunderstanding on my part, but whenever I try to run a __main__.py file within a workspace that imports its own package I get a "is not a package" error.

command:
```
uv run printer/src/printer "me"     
                         
Traceback (most recent call last):
  File "/home/anders/.local/share/mise/installs/python/3.10.16/lib/python3.10/runpy.py", line 196, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "/home/anders/.local/share/mise/installs/python/3.10.16/lib/python3.10/runpy.py", line 86, in _run_code
    exec(code, run_globals)
  File "/home/anders/dev/uvmain/printer/src/printer/__main__.py", line 2, in <module>
    from printer.printer import hello
ModuleNotFoundError: No module named 'printer.printer'; 'printer' is not a package
```

I created the workspace by doing (ish):
```
mkdir printtest && cd printtest
uv init
uv init --lib printer
uv add ./printer
```


layout:
```
.
├── README.md
├── main.py
├── printer
│   ├── README.md
│   ├── pyproject.toml
│   └── src
│       └── printer
│           ├── __init__.py
│           ├── __main__.py
│           ├── __pycache__
│           │   ├── __init__.cpython-310.pyc
│           │   ├── __main__.cpython-310.pyc
│           │   └── printer.cpython-310.pyc
│           ├── printer.py
│           └── py.typed
├── pyproject.toml
└── uv.lock
```

`pyproject.toml`:
```
[project]
name = "uvmain"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
  "printer",
]

[tool.uv.workspace]
members = ["printer"]

[tool.uv.sources]
printer = { workspace = true }
```

`printer/pyproject.toml`:
```
[project]
name = "printer"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "andersfylling", email = "email@email.com" }
]
requires-python = ">=3.10"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

`printer/src/printer/hello.py`:
```
def hello(value: str) -> str:
    return f"Hello {value} from printer!"
````

`printer/src/printer/__main__.py`:
```
import sys
from printer.printer import hello

if __name__ == "__main__":
    hello(sys.argv[1])
```

I quite like the idea of having a `__main__.py` with CLI logic to help onboard a new user, as it allows them interact with the package logic. Is there something obvious I'm doing wrong?

### Platform

Linux 6.13.5-arch1-1 x86_64 GNU/Linux

### Version

uv 0.6.4 (04db70662 2025-03-03)

---

_Label `question` added by @andersfylling on 2025-03-11 16:46_

---

_Comment by @konstin on 2025-03-12 09:57_

You need to use `uv run -m printer`, otherwise `import printer` will load `printer/src/printer/printer.py` (the file next to the one you're running) instead of the full module.

---

_Comment by @andersfylling on 2025-03-13 08:54_

Yes, that solves it. Thank you!

---

_Closed by @andersfylling on 2025-03-13 08:54_

---
