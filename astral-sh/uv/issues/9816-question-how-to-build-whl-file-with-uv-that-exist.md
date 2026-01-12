```yaml
number: 9816
title: "[QUESTION] How to build whl file with uv that exist same name in pypi??"
type: issue
state: closed
author: hawksung1
labels:
  - question
assignees: []
created_at: 2024-12-11T16:16:27Z
updated_at: 2024-12-13T16:30:51Z
url: https://github.com/astral-sh/uv/issues/9816
synced_at: 2026-01-12T15:59:59Z
```

# [QUESTION] How to build whl file with uv that exist same name in pypi??

---

_@hawksung1_

I am planning to create a project that uses a local .whl file with uv. 
However, there is a .whl file with the same name available on PyPI. 
For example, our pyproject.toml looks like this:


```
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "langflow-base",
]

[tool.uv.sources]
langflow-base = { path = "../../../tmp/langflow_base-0.1.1-py3-none-any.whl" }

```


After building the project with uv, when I try to install the generated .whl file from another location, it downloads the file from PyPI instead.
How can I configure the build process to prioritize the local .whl file?

Thanks for solving my problem

---

_Comment by @zanieb on 2024-12-11 18:21_

A built package can't contain references to other wheels via a relative file path, that's sort of "by definition". 

At install time, you'd need to add an index containing a wheel for the package, e.g., with `--find-links <directory>`

---

_Label `question` added by @zanieb on 2024-12-11 18:21_

---

_Comment by @zanieb on 2024-12-11 18:21_

I'd recommend picking a unique name to avoid confusion though.

---

_Closed by @hawksung1 on 2024-12-13 16:14_

---

_Comment by @hawksung1 on 2024-12-13 16:30_

Thank you so much. I solved my problem using --find-links

---
