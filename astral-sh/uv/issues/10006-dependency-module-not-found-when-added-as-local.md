```yaml
number: 10006
title: dependency module not found when added as local file path
type: issue
state: closed
author: straz
labels:
  - question
  - needs-mre
assignees: []
created_at: 2024-12-18T16:18:01Z
updated_at: 2024-12-20T03:16:42Z
url: https://github.com/astral-sh/uv/issues/10006
synced_at: 2026-01-10T04:36:21Z
```

# dependency module not found when added as local file path

---

_Issue opened by @straz on 2024-12-18 16:18_

In `pyproject.toml`:

```
[project]
name = "myproject"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
    "my_base_project",
    "fastapi>=0.115",
    "httpx>=0.28",
    "networkx>=3.4.2",
    "pydantic>=2.10",
    "pyyaml>=6.0.2",
    "requests>=2.32",
    "uvicorn>=0.32.0",
]

[tool.uv.sources]
my_base_project = { path = "${PROJECT_ROOT}/../../../mybase/src/" }
```

When I run my app, the base dependency does not appear and cannot be loaded:
```
  % uv run uvicorn main:app --host 0.0.0.0 --port 8000 --reload
  ..
    from my_base_project.schemas import (
ModuleNotFoundError: No module named 'my_base_project'
```

According to `uv run --verbose`, it seems like `my_base_project` has been loaded as a requirement. But it does not appear anywhere in `sys.path`. Shouldn't I expect to be able to `import my_base_project`, and if so, from where?
```
% uv run --verbose uvicorn main:app
DEBUG Using request timeout of 30s
DEBUG Requirement already installed: my_base_project==0.1.0 (from file:///Users/straz/src/mybase/src)
DEBUG Requirement already installed: annotated-types==0.7.0
DEBUG Requirement already installed: anyio==4.7.0
DEBUG Requirement already installed: black==24.10.0
DEBUG Requirement already installed: cachetools==5.5.0
DEBUG Requirement already installed: certifi==2024.12.14
...
```

---

_Comment by @charliermarsh on 2024-12-18 17:12_

I suspect something is wrong in how you setup `my_base_project`. It's not typical to point to a `src` directory like that.

---

_Label `question` added by @charliermarsh on 2024-12-18 17:29_

---

_Label `needs-mre` added by @charliermarsh on 2024-12-18 17:29_

---

_Comment by @charliermarsh on 2024-12-18 17:29_

To help you, we would need to see a complete reproduction, such as a Git repository and a series of commands we can use to reproduce.

---

_Comment by @nrydanov on 2024-12-18 18:42_

Same issue, but I add project directly via CLI like that: `uv add --editable ../shared` where `../shared` is a folder with correct `pyproject.toml` inside it.

Project is named inbrief-shared, but none of 
`import shared`
`import inbrief_shared` 
works

In `.venv/lib/python3.12/site-packages` I see that UV indeed does smth with that module. There's .pth file, for example. However, I can't see folder with .py files inside .venv as I do when open regular dependencies folders.

And I can't use this package in runtime...

---

_Comment by @charliermarsh on 2024-12-18 18:48_

I suspect there's an issue with how your package is structured. I'd need a reproduction, like a Git repository, to help out.

---

_Comment by @nrydanov on 2024-12-18 18:49_

Ok, will send soon.

---

_Comment by @nrydanov on 2024-12-18 19:11_

It seems like I found an issue. I was reusing .venv that were left from Rye after migration to UV. 

Deleting .venv and `uv sync` solved the problem for me. Thanks for response and sorry for your time wasting.

---

_Comment by @straz on 2024-12-18 20:33_

Thank you @nrydanov and @charliermarsh. I've pushed some sample code to https://github.com/straz/uv-issue-10006.

This seems to find `static pyproject.toml for: base-project`, but still doesn't include it in the python path.

```shell
mac % cd uv_example/my_project
mac % uv run --verbose main.py
DEBUG uv 0.5.0 (Homebrew 2024-11-07)
DEBUG Found project root: `/Users/straz/src/uv_example/my_project`
DEBUG No workspace root found, using project root
DEBUG Discovered project `my-project` at: /Users/straz/src/uv_example/my_project
DEBUG Using Python request `>=3.11` from `requires-python` metadata
DEBUG The virtual environment's Python version satisfies `>=3.11`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: my-project @ file:///Users/straz/src/uv_example/my_project
DEBUG No workspace root found, using project root
DEBUG Found static `pyproject.toml` for: base-project @ file:///Users/straz/src/uv_example/base_project
DEBUG Project is contained in non-workspace project: `/Users/straz/src/uv_example/my_project`
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 23 packages in 0.57ms
...
Audited 20 packages in 0.03ms
DEBUG Using Python 3.11.6 interpreter at: /Users/straz/src/uv_example/my_project/.venv/bin/python3
DEBUG Running `python main.py`
Traceback (most recent call last):
  File "/Users/straz/src/uv_example/my_project/main.py", line 2, in <module>
    from my_thing import MyThing
  File "/Users/straz/src/uv_example/my_project/my_thing.py", line 1, in <module>
    from base_project.main import Base
ModuleNotFoundError: No module named 'base_project'
DEBUG Command exited with code: 1
```

---

_Comment by @jannik4 on 2024-12-19 22:06_

I am hitting the same problem, I think. I created a reproduction [here](https://github.com/jannik4/uv-local-dep/tree/fb155aa57c77afe11428e05dbc9c8f9a575e0ea8).

Edit:
[Fixed it here.](https://github.com/jannik4/uv-local-dep/commit/47bf13b9ded2196e6c06d4e9b320f2d30c09dc83) (But I think the error is still happening at the wrong time, i.e. when running mybin not when adding the dependency.)

---

_Comment by @charliermarsh on 2024-12-20 03:16_

Yeah, if you don't have a build system, we won't build and install your project in the non-`uv pip` APIs.

Check out: https://docs.astral.sh/uv/concepts/projects/config/#build-systems.

---

_Closed by @charliermarsh on 2024-12-20 03:16_

---
