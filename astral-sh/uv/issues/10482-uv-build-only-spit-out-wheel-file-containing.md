---
number: 10482
title: uv build only spit out wheel file containing metadata but no code
type: issue
state: open
author: tmatup
labels:
  - question
assignees: []
created_at: 2025-01-10T22:41:10Z
updated_at: 2025-01-10T23:27:53Z
url: https://github.com/astral-sh/uv/issues/10482
synced_at: 2026-01-10T01:24:54Z
---

# uv build only spit out wheel file containing metadata but no code

---

_Issue opened by @tmatup on 2025-01-10 22:41_

`uv build` command is generating a very small wheel file, which is only including essential metadata and minimal code. what could be missing?

the following is the conten of the `pyproject.toml` file
```
[project]
name = "my-project"
version = "0.1.0"
description = "My Project"
authors = []
readme = "README.md"
requires-python = ">=3.11,<3.12"
packages = [
    { include = "root/my_project" }
]dependencies = [
    # FROZEN DEPENDENCIES - FOR PRODUCTION
    ... 
    # UPDATEABLE DEPENDENCIES - FOR TESTS, EVALATION
    ...
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["root/my_project"]

[tool.uv]
package = false # force the project package (gpt-tasks) not to be built and installed into the project environment
```

then I do `uv build` under the `root/my_project` folder where the pyproject.toml is sitting, 

```
:~/myrepro/root/my_project$ uv build
Building source distribution...
Building wheel from source distribution...
Successfully built dist/my_project-0.1.0.tar.gz
Successfully built dist/my_project-0.1.0-py3-none-any.whl
:~/myrepro/root/my_project$ ls -la dist/
total 17008
drwxrwxr-x  2 tao tao     4096 Jan 10 14:34 .
drwxrwxr-x 12 tao tao     4096 Jan 10 14:24 ..
-rw-rw-r--  1 tao tao        1 Jan 10 14:24 .gitignore
-rw-r--r--  1 tao tao     2023 Jan 10 14:34 my_project-0.1.0-py3-none-any.whl
-rw-r--r--  1 tao tao 17398499 Jan 10 14:34 my_project-0.1.0.tar.gz
```

as you can see, the wheel is pretty small, unzipping it showed that it only contains 3 files - METADATA, RECORD and WHEEL, but none of the code under the root/my_project folder. 

---

_Comment by @zanieb on 2025-01-10 23:27_

Looks like you're configuring hatch to find packages in `root/myproject` but you're building the wheel from `root/myproject`?

---

_Label `question` added by @zanieb on 2025-01-10 23:27_

---
