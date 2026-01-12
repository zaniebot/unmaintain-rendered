```yaml
number: 12900
title: python_full_version does not match pyproject.toml and .python-version
type: issue
state: closed
author: oseymour
labels:
  - question
assignees: []
created_at: 2025-04-15T17:32:23Z
updated_at: 2025-04-17T12:33:57Z
url: https://github.com/astral-sh/uv/issues/12900
synced_at: 2026-01-12T16:01:15Z
```

# python_full_version does not match pyproject.toml and .python-version

---

_@oseymour_

### Question

I'm working on project which requires mmsegmentation and mmcv. With the `pyproject.toml` below, I can run `uv sync` and then `uv add mmcv==2.0.0 -f https://download.openmmlab.com/mmcv/dist/cpu/torch2.0/index.html` and mmcv 2.0.0 is successfully installed into the environment. `.python-version` contains "3.10".

``` toml
[project]
name = "usfm"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "mmsegmentation>=1.2.2",
    "torch==2.0.1",
]

[tool.uv]
allow-insecure-host = ["https://download.openmmlab.com/mmcv/dist"]
```

I'd like to set up the project to install everything with just `uv sync` and modified the `pyproject.toml` to this:
``` toml
[project]
name = "usfm"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "mmcv==2.0.0",
    "mmsegmentation>=1.2.2",
    "torch==2.0.1",
]

[tool.uv]
allow-insecure-host = ["https://download.openmmlab.com/mmcv/dist"]

[tool.uv.sources]
mmcv = { index = "mmcv" }

[[tool.uv.index]]
name = "mmcv"
url = "https://download.openmmlab.com/mmcv/dist/cpu/torch2.0/index.html"
```

But when I run `uv sync` I get this error:
![Image](https://github.com/user-attachments/assets/e26cda70-3921-43eb-b2e9-9a02b442330b)

**My question is:** Why is this error happening? I've specified Python 3.10. In the error message image I can see that Python 3.10 is being used to initialize the venv. Why is the resolver checking against Python 3.12?

### Platform

Windows 10 x64

### Version

uv 0.6.2 (6d3614eec 2025-02-19)

---

_Label `question` added by @oseymour on 2025-04-15 17:32_

---

_Comment by @charliermarsh on 2025-04-15 18:25_

A few things:

1. We resolve for all supported Python versions, not just the "current" version. Since your project supports `>=3.10`, we still have to produce a successful resolution for Python 3.12.
2. I think that's a red-herring. `https://download.openmmlab.com/mmcv/dist/cpu/torch2.0/index.html` isn't a valid Python package registry as-specified. You might want to try adding `format = "flat"` under that `[[tool.uv.index]]` entry. See: https://docs.astral.sh/uv/configuration/indexes/#flat-indexes.

---

_Comment by @charliermarsh on 2025-04-17 12:33_

Closing, but happy to re-open if there are follow-up questions.

---

_Closed by @charliermarsh on 2025-04-17 12:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-17 12:33_

---
