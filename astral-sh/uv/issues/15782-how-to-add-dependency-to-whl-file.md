---
number: 15782
title: How to add dependency to whl file
type: issue
state: closed
author: SimonJonsson1999
labels:
  - question
assignees: []
created_at: 2025-09-11T07:25:20Z
updated_at: 2025-09-12T15:05:57Z
url: https://github.com/astral-sh/uv/issues/15782
synced_at: 2026-01-10T01:25:59Z
---

# How to add dependency to whl file

---

_Issue opened by @SimonJonsson1999 on 2025-09-11 07:25_

### Question

Hello, I have recently switched over to uv, and I would like some guidance for best practices.

I am managing a private python package using uv. This package depends on a whl file that is not find on PYPI or similar online, I have created it locally. 
Here is what i want, but i am not sure how to actually make it work.
```
[project]
name = "name"
version = "0.1.0"
description = "description"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "whl file",
    "package depending on whl file>=0.6.0"
]


[build-system]
requires = ["uv_build>=0.5", "uv>=0.8"]
build-backend = "uv_build"

```

I need someone to be able to 

> pip install git+ssh://git@repo@tag

and this needs to install the whl file, the package depending on the whl file and my package.

### Platform

Windows11

### Version

uv 0.8.13 (ede75fe62 2025-08-21)

---

_Label `question` added by @SimonJonsson1999 on 2025-09-11 07:25_

---

_Comment by @konstin on 2025-09-11 07:43_

This is unfortunately not properly supported by the wheel file format. The closest you can do is `package_name @ https://example.com/path/to/wheel/pkg-1-py3-none-any.whl` as requirement.

---

_Comment by @SimonJonsson1999 on 2025-09-11 11:36_

Okay thank you.
Out of curiosity, why is this not supported? Technical reason or because not many people want it? 

---

_Comment by @konstin on 2025-09-11 13:22_

Most registries have the rules that you can only point to other entries in the registry, for consistency and for security. As most people use registries, design and features are centered around them, and URL support exists mainly for specific use cases, mostly around pulling in source code instead of wheels, such as replacing a dependency with a Git repository in an application. 

---

_Closed by @charliermarsh on 2025-09-12 15:05_

---
