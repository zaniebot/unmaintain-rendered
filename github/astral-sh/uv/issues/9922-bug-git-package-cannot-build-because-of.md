---
number: 9922
title: "bug: git package cannot build, because of ModuleNotFoundError: No module named 'requests'"
type: issue
state: closed
author: qiansen1386
labels:
  - question
assignees: []
created_at: 2024-12-15T22:02:50Z
updated_at: 2024-12-17T07:04:57Z
url: https://github.com/astral-sh/uv/issues/9922
synced_at: 2026-01-07T13:12:18-06:00
---

# bug: git package cannot build, because of ModuleNotFoundError: No module named 'requests'

---

_Issue opened by @qiansen1386 on 2024-12-15 22:02_

I was trying to install a forked pip package, e.g. `uv pip install git+https://github.com/qiansen1386/lixinger-openapi.git` which is forked from `lixinger-openapi`
but I got `ModuleNotFoundError: No module named 'requests'` What buzz me is I do have requests in my venv(double checked via python import)

Here is my project.toml
```toml
dependencies = [
    "jupyter>=1.1.1",
    "matplotlib>=3.9.3",
    "numpy>=2.1.0",
    "pandas>=2.2.3",
    "requests>=2.32.3",
...
]
requires-python = ">= 3.13"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.rye]
managed = true
dev-dependencies = ["ruff>=0.7.4"]

[tool.hatch.metadata]
allow-direct-references = true

[tool.hatch.build.targets.wheel]
packages = ["src"]
```
Error: 
```
$ uv pip install git+https://github.com/qiansen1386/lixinger-openapi.git
 Updated https://github.com/qiansen1386/lixinger-openapi.git (23e43f7)
error: Build backend failed to determine requirements with `build_wheel()` (exit code: 1)                                                

[stderr]
Traceback (most recent call last):
  File "<string>", line 14, in <module>
    requires = get_requires_for_build({})
...
  File "%USERPROFILE%l\uv\cache\git-v0\checkouts\3222d26cf6fece48\23e43f7\lixinger_openapi\query.py", line 6, in <module>  
    import requests
ModuleNotFoundError: No module named 'requests'
```
I have no clue.. How come `build_wheel()` cannot find requests 😢 

---

_Renamed from "bug" to "bug: git package cannot build, because of ModuleNotFoundError: No module named 'requests'" by @qiansen1386 on 2024-12-15 22:03_

---

_Comment by @charliermarsh on 2024-12-15 22:12_

This is an issue with the package itself. It's trying to import `requests` while building, but `requests` isn't declared as a build dependency.

---

_Label `question` added by @charliermarsh on 2024-12-15 22:14_

---

_Closed by @qiansen1386 on 2024-12-16 17:42_

---

_Comment by @frank-miao on 2024-12-17 07:04_

Hi, I encountered a similar problem. Could you please tell me how it was resolved?

---
