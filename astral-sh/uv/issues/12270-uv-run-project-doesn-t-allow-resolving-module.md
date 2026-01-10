---
number: 12270
title: "`uv run --project` doesn't allow resolving module imports on windows"
type: issue
state: closed
author: leokeba
labels:
  - bug
assignees: []
created_at: 2025-03-18T10:34:30Z
updated_at: 2025-04-15T12:45:42Z
url: https://github.com/astral-sh/uv/issues/12270
synced_at: 2026-01-10T01:25:17Z
---

# `uv run --project` doesn't allow resolving module imports on windows

---

_Issue opened by @leokeba on 2025-03-18 10:34_

### Summary

In my python project, I have a `scripts` folder where I keep various utilities that need to import code from the main `app` folder. When running it on macOS, I just use `uv run scripts/test.py --project .` and everything works as expected. On my windows system, when I try `uv run scripts\test.py --project .` i get the following error : `ModuleNotFoundError: No module named 'app'`.

I tried using an absolute path instead and various tricks to make it work, and also using the `--folder` argument instead, but nothing helps. Am I misunderstanding something or is this a bug ?

### Platform

Windows 11 x86_64

### Version

0.6.6

### Python version

3.13.2

---

_Label `bug` added by @leokeba on 2025-03-18 10:34_

---

_Renamed from "`uv run --project` doesn't allow resolving module imports" to "`uv run --project` doesn't allow resolving module imports on windows" by @leokeba on 2025-03-18 10:48_

---

_Comment by @nbaju1 on 2025-03-18 12:45_

Isn't `--project .` the same as running without the flag, since the default behavior is to look for the project within the current directory? If so do you get the same result without it? 

---

_Comment by @leokeba on 2025-03-18 13:07_

> Isn't `--project .` the same as running without the flag, since the default behavior is to look for the project within the current directory? If so do you get the same result without it?

On macOS, running a script from a subfolder breaks module imports from the rest of the app as the python interpreter runs from the subfolder environment instead of the root project folder. The solution I found to this issue was using the `--project .` flag, but this only works on macOS.

On windows, both give the same result, which is the following error : `ModuleNotFoundError: No module named 'app'`

In my opinion, the best solution would be not needing to use the `--project .` flag at all regardless of the OS or subfolder structure, but I figure there might be a reason things work that way.

---

_Comment by @nbaju1 on 2025-03-18 13:22_

Not able to reproduce on Windows 11.

Folder structure:
```markdown
project/
├── app/
|   └── foo.py
├── scripts/
|   └── test.py
└── pyproject.toml
```

foo.py
```python
def bar():
    print("bar")
```

test.py
```python
from app.foo import bar

bar()
```

Running `uv run .\scripts\test.py` (without or without `--project .`) from the root outputs `bar`.

---

_Comment by @leokeba on 2025-03-18 13:51_

I reproduced your example and still get the same error on Windows 11.

What are the contents of your pyproject.toml ?

---

_Comment by @nbaju1 on 2025-03-18 13:53_

pyproject.toml
```toml
[project]
name = "project"
version = "0.1.0"
description = "Project"
requires-python = ">=3.12"
````

Note that I have placed the venv inside the project root and it is active before running the command, if that matters.
Edit: Scratch that. Deleted the venv and recreated it, now I get the same error as you. Old venv on my end it seems.

---

_Comment by @leokeba on 2025-03-18 14:01_

The only difference is that I was using python 3.13 instead of 3.12, but i tried changing the version and I still get the same result.

I am using a .venv placed in the project root as well. Any ideas for a workaround ?

---

_Comment by @nbaju1 on 2025-03-18 14:05_

Adding this to the pyproject.toml file 

```toml
[project.scripts]
test = "scripts.test:main"

[build-system]
requires = ["uv_build"]
build-backend = "uv_build"

[tool.uv.build-backend]
module-root = ""
module-name = "app"
```
(placing the `bar()` call in a `main` function in test.py) and running `uv run test` works though (in uv v0.6.7)

---

_Comment by @leokeba on 2025-03-18 14:14_

Thanks a lot, this solution does indeed fix all my issues on both OSes, and it is much more elegant than what I was doing before.

You can close this issue if everything else is expected behaviour.

---

_Closed by @konstin on 2025-04-15 12:45_

---
