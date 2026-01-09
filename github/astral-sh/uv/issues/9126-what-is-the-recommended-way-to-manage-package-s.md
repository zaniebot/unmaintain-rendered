---
number: 9126
title: "What is the recommended way to manage package's version"
type: issue
state: open
author: maxkoretskyi
labels:
  - question
assignees: []
created_at: 2024-11-14T18:00:38Z
updated_at: 2024-11-14T18:35:55Z
url: https://github.com/astral-sh/uv/issues/9126
synced_at: 2026-01-07T13:12:18-06:00
---

# What is the recommended way to manage package's version

---

_Issue opened by @maxkoretskyi on 2024-11-14 18:00_

I'm using `pyproject.toml` to define package metadata:

```
[build-system]
requires = ["setuptools>=68"]
build-backend = "setuptools.build_meta"

[project]
name = "myp"
requires-python = ">=3.11"
version = "0.19.1b4"      <--------------------------------------------- <here>
...
```

I can put the package's version (in this case `0.19.1b4`) directly into the `pyproject.toml`, but are there more sophisticated scenarios supported by `uv`? 

Something like this maybe

```
[project]
dynamic = ["version"]

[tool.setuptools.dynamic]
version = { attr = "mylib.__version__" }
```

or perhaps there's a built-in tooling for version management.


---

_Comment by @zanieb on 2024-11-14 18:25_

A dynamic version is problematic with our design, for discussion see #7533 

Dynamic metadata in general isn't recommended. It adds a great deal of complexity to the packaging ecosystem. I'd recommend just using a static version, that's what we do here in this project. There's an issue at https://github.com/astral-sh/uv/issues/6298 tracking addition of a command to update the version programatically. Lots more context and discussion there.

---

_Label `question` added by @zanieb on 2024-11-14 18:25_

---

_Comment by @maxkoretskyi on 2024-11-14 18:35_

great, thanks for the links, I'll explore 

---
