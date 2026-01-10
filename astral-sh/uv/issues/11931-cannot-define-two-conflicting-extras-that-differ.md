---
number: 11931
title: "Cannot define two conflicting extras that differ by \"+cpu\""
type: issue
state: closed
author: vmarkovtsev
labels:
  - question
assignees: []
created_at: 2025-03-03T18:24:24Z
updated_at: 2025-03-03T18:34:45Z
url: https://github.com/astral-sh/uv/issues/11931
synced_at: 2026-01-10T01:25:12Z
---

# Cannot define two conflicting extras that differ by "+cpu"

---

_Issue opened by @vmarkovtsev on 2025-03-03 18:24_

### Summary

Given the following `pyproject.toml`, which tries to support `pip install project[cuda]` XOR `pip install project[cpu]`, `uv lock` fails.
```
[project]
name = "project"
requires-python = ">= 3.10"
dependencies = [
    "vllm==0.7.3",
]
version = "1.0.0"

[project.optional-dependencies]
cuda = [
    "torch==2.5.1",
]
cpu = [
    "torch==2.5.1+cpu",
]

[tool.uv]
conflicts = [
    [
        { extra = "cuda" },
        { extra = "cpu" },
    ],
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "torch-gpu"
url = "https://download.pytorch.org/whl/cu126"
explicit = true

[tool.uv.sources]
torch = [
    { index = "torch-gpu", extra = "cuda" },
    { index = "torch-cpu", extra = "cpu" },
]
```

Message from `uv lock`:
```
Using CPython 3.12.8
  × No solution found when resolving dependencies for split (python_full_version >= '3.12'):
  ╰─▶ Because there is no version of torch==2.5.1 and project[cuda] depends on torch==2.5.1, we can conclude that project[cuda]'s requirements are unsatisfiable.
      And because your project requires project[cuda], we can conclude that your project's requirements are unsatisfiable.
```
```

### Platform

Ubuntu 22.04

### Version

0.6.3

### Python version

3.10

---

_Label `bug` added by @vmarkovtsev on 2025-03-03 18:24_

---

_Comment by @charliermarsh on 2025-03-03 18:26_

It looks like 2.5.1 does not exist on the cu126 index: https://download.pytorch.org/whl/cu126/torch

---

_Label `bug` removed by @charliermarsh on 2025-03-03 18:26_

---

_Label `question` added by @charliermarsh on 2025-03-03 18:26_

---

_Comment by @vmarkovtsev on 2025-03-03 18:34_

I don't believe that was so stupid 🤦 Thank you so much.

---

_Closed by @vmarkovtsev on 2025-03-03 18:34_

---
