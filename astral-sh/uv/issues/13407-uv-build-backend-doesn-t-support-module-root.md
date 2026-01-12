```yaml
number: 13407
title: "uv build backend doesn't support ` module-root = \"./\"`"
type: issue
state: closed
author: followheart26
labels:
  - bug
  - build-backend
assignees: []
created_at: 2025-05-12T11:37:55Z
updated_at: 2025-05-15T15:19:04Z
url: https://github.com/astral-sh/uv/issues/13407
synced_at: 2026-01-12T16:01:27Z
```

# uv build backend doesn't support ` module-root = "./"`

---

_@followheart26_

### Question

i have a dir backend-sdk and the structure like 
backend-sdk
 -- mybackend
 -- pyproject.toml
all source code in dir `mybackend` and it include a __init__.py

and my pyproject.toml content like this
[tool.uv.build-backend]
module-root = "./"
module-name = "mybackend"

i run `uv build` in the backend-sdk dir

the error log is
Error: Expected a Python module directory at: `mybackend`

### Platform

centos

### Version

latest

---

_Label `question` added by @followheart26 on 2025-05-12 11:37_

---

_Comment by @konstin on 2025-05-12 11:40_

Is the directory you are using called `backend` or `mybackend`? Ideally, could you share a Git repository that shows the problem? Not with your entire code, just with the files needed for the error to happen.

---

_Comment by @followheart26 on 2025-05-12 11:53_

> Is the directory you are using called `backend` or `mybackend`? Ideally, could you share a Git repository that shows the problem? Not with your entire code, just with the files needed for the error to happen.

just empty dirs with file structure i give
.
├── mybackend
│   └── __init__.py
└── pyproject.toml

---

_Comment by @followheart26 on 2025-05-12 11:54_

> Is the directory you are using called `backend` or `mybackend`? Ideally, could you share a Git repository that shows the problem? Not with your entire code, just with the files needed for the error to happen.

the all content of pyproject.toml is
[build-system]
requires = ["uv_build>=0.7.3,<0.8.0"]
build-backend = "uv_build"

[tool.uv.build-backend]
module-root = "./"
module-name = "mybackend"

[project]
name = "mybackend"
version = "1.3.1a1"
description = "mybackend SDK"
dependencies = []

---

_Comment by @konstin on 2025-05-12 11:56_

It works with `module-root = ""`, we should accept `./`, too, though.

---

_Label `question` removed by @konstin on 2025-05-12 11:56_

---

_Label `bug` added by @konstin on 2025-05-12 11:56_

---

_Renamed from "how to set the module-root and module-name" to "uv build backend doesn't support ` module-root = "./"`" by @konstin on 2025-05-12 11:57_

---

_Assigned to @konstin by @konstin on 2025-05-12 11:57_

---

_Label `build-backend` added by @konstin on 2025-05-12 11:57_

---

_Comment by @followheart26 on 2025-05-13 02:18_

> It works with `module-root = ""`, we should accept `./`, too, though.

i use module-root = "", and i find that if the source code dir named like nBk tCk  also raise the Error: Expected a Python module directory at: `tCk`, can't i use capital letter in source code dir? 

---

_Closed by @konstin on 2025-05-15 15:19_

---
