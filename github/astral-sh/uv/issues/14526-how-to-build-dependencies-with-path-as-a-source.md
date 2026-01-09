---
number: 14526
title: How to build dependencies with path as a source
type: issue
state: open
author: mauriciocm9
labels:
  - question
assignees: []
created_at: 2025-07-09T16:52:47Z
updated_at: 2025-07-25T20:10:14Z
url: https://github.com/astral-sh/uv/issues/14526
synced_at: 2026-01-07T13:12:18-06:00
---

# How to build dependencies with path as a source

---

_Issue opened by @mauriciocm9 on 2025-07-09 16:52_

### Question

uv build currently does not build wheels for local path dependencies such as ../B. This limits its usefulness in monorepo or sibling-package development workflows.

```
/my-project/
├── A/
│   ├── pyproject.toml  # depends on ../B
└── B/
    ├── pyproject.toml
```

A pyproject would look like:
```toml
[project]
name = "A"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "B",
]

[tool.uv.sources]
B = { path = "../B", editable = true }
```

I was wondering if there is a way to automatically create a build for local dependencies. I know using workspaces works perfectly using `uv build --all-packages`. But I cannot always use workspaces due to conflicting requirements.

My current workflow is to export dependencies using `uv pip freeze --exclude-editable` and then read the pyproject.toml to know which packages must be built.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @mauriciocm9 on 2025-07-09 16:52_

---

_Comment by @zanieb on 2025-07-09 16:56_

> But I cannot always use workspaces due to conflicting requirements.

Can you say more about this? We can add the ability to let workspace members have conflicting packages, as we have for groups and extras.

---

_Comment by @mauriciocm9 on 2025-07-09 17:12_

> > But I cannot always use workspaces due to conflicting requirements.
> 
> Can you say more about this? We can add the ability to let workspace members have conflicting packages, as we have for groups and extras.

I was having issues with tensorflow and pytorch. Tensorflow2 is supported with python versions 3.6-3.9 but latest pytorch versions requires  >=3.9. I think the tensorflow model i was trying to use required a python version of 3.8

---

_Comment by @lukaskeller on 2025-07-25 19:57_

Allowing for conflicting requirements gracefully within workspaces would be amazing for larger ML monorepos

---

_Comment by @zanieb on 2025-07-25 20:10_

Working on that in https://github.com/astral-sh/uv/pull/14906

---
