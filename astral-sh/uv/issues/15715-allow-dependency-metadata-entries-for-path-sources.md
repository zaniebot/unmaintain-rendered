---
number: 15715
title: Allow dependency metadata entries for path sources
type: issue
state: open
author: nathanjmcdougall
labels:
  - enhancement
assignees: []
created_at: 2025-09-08T01:40:26Z
updated_at: 2025-09-08T02:10:04Z
url: https://github.com/astral-sh/uv/issues/15715
synced_at: 2026-01-10T01:25:58Z
---

# Allow dependency metadata entries for path sources

---

_Issue opened by @nathanjmcdougall on 2025-09-08 01:40_

### Summary

As far as I can tell, `tool.uv.dependency-metadata` are not supported for Path sources.

For some motivation, I have a piece of proprietary software which installs a wheel file at a specific location in Windows, say `C:/Program Files/ProprietaryFoo/foo-0.1.0-py3-none-any.whl`. I'd like it to be an optional dependency for the project so I have

```TOML
[project]
optional-dependencies.foo = [ "foo; sys_platform=='win32'" ]

[tool.uv.sources]
foo = { path = "C:/Program Files/ProprietaryFoo/foo-0.1.0-py3-none-any.whl", marker = "sys_platform == 'win32'" }
```

This works fine locally, but then on CI running Linux, the resolver will try to access the path, which doesn't exist, similar to #8389.

As a temporary workaround, I'm just going to create a dummy `.whl` at that location on the CI. 

### Example

Ideally, I'd like to be able to declare `dependency-metadata` over-rides to make it possible to avoid resolving this path when the markers don't match. For example:

```TOML
[[tool.uv.dependency-metadata]]
name = "foo"
version = "0.1.0"
requires-dist = [ "protobuf<6.0.0,>=5.29.0" ]
```

However, as it stands, this doesn't seem to have any effect.

Perhaps as an aside, it would also be nice if the dependency over-rides kicked in even when the markers match (e.g. if the proprietary software wasn't installed on Windows). I don't think it should be necessary to access the wheel file unless actually trying to install the optional dependency (it's optional, after all).

---

_Label `enhancement` added by @nathanjmcdougall on 2025-09-08 01:40_

---

_Comment by @charliermarsh on 2025-09-08 01:45_

It is supported for direct URL dependencies. Can you include verbose logs or a minimal reproduction?

---

_Comment by @nathanjmcdougall on 2025-09-08 02:09_

This is for a (local) path dependency (https://docs.astral.sh/uv/concepts/projects/dependencies/#path), not a URL dependency. 

In terms of a minimal reproduction, try setting `pyproject.toml` to the following and running on Linux:

```TOML
[project]
name = "demo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []
optional-dependencies.foo = [ "foo; sys_platform=='win32'" ]

[tool.uv.sources]
foo = { path = "C:/Program Files/ProprietaryFoo/foo-0.1.0-py3-none-any.whl", marker = "sys_platform == 'win32'" }

[[tool.uv.dependency-metadata]]
name = "foo"
version = "0.1.0"
requires-dist = [ "protobuf<6.0.0,>=5.29.0" ]
```

I would expect `uv sync` to run successfully on Linux in this case, since the `foo` dependency has its metadata declared explicitly, and resolving from the (non-existent) path shouldn't be necessary.

Similarly, on Windows, the following `pyproject.toml` should reproduce:

<details>

```TOML
[project]
name = "demo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []
optional-dependencies.foo = [ "foo; sys_platform=='linux'" ]

[tool.uv.sources]
foo = { path = "/foo-0.1.0-py3-none-any.whl", marker = "sys_platform == 'linux'" }

[[tool.uv.dependency-metadata]]
name = "foo"
version = "0.1.0"
requires-dist = [ "protobuf<6.0.0,>=5.29.0" ]
```

</details>

Which gives the following log when running `uv sync -vvv`:

<details>

```
DEBUG uv 0.7.13 (62ed17b23 2025-06-12)
DEBUG Found project root: `C:\Users\namc\Temp\demo`
DEBUG No workspace root found, using project root
TRACE Checking lock for `C:\Users\namc\Temp\demo` at `C:\Users\namc\AppData\Local\Temp\uv-d9d139ad2fabec31.lock`
DEBUG Acquired lock for `C:\Users\namc\Temp\demo`
DEBUG Reading Python requests from version file at `C:\Users\namc\Temp\demo\.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.13.2, skipping probing: .venv\Scripts\python.exe
DEBUG The project environment's Python version satisfies the request: `Python 3.13`
TRACE The project environment's Python version meets the Python requirement: `>=3.13`
DEBUG Released lock at `C:\Users\namc\AppData\Local\Temp\uv-d9d139ad2fabec31.lock`
TRACE Checking lock for `.venv` at `.venv\.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: demo @ file:///C:/Users/namc/Temp/demo
DEBUG No workspace root found, using project root
TRACE Performing lookahead for demo[foo] @ file:///C:/Users/namc/Temp/demo
TRACE Performing lookahead for foo @ file:///C:/foo-0.1.0-py3-none-any.whl ; sys_platform == 'linux' and extra == 'foo'
DEBUG Released lock at `C:\Users\namc\Temp\demo\.venv\.lock`
error: Distribution not found at: file:///C:/foo-0.1.0-py3-none-any.whl
```

</details>



---
