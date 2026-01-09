---
number: 15069
title: "uv Can't find a correct candidate for package pywin32"
type: issue
state: open
author: sanzenwin
labels:
  - bug
assignees: []
created_at: 2025-08-04T23:29:11Z
updated_at: 2025-08-06T04:14:03Z
url: https://github.com/astral-sh/uv/issues/15069
synced_at: 2026-01-07T13:12:19-06:00
---

# uv Can't find a correct candidate for package pywin32

---

_Issue opened by @sanzenwin on 2025-08-04 23:29_

### Summary

I have `Python 3.10.11`(contains uv) and `Python 3.7.6` on my machine,  the result should be `pywin32-308-cp37-cp37m-win_amd64.whl`, but it give me `pywin32==311`
<br>
pyproject.toml

```
[project]
name = "testtest"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.7"
dependencies = [
    "pywin32"
]
```
<br>
<br>

`uv sync -vvv` [uv.log](https://github.com/user-attachments/files/21585974/uv.log)


### Platform

Windows 10 x86_64

### Version

uv 0.8.4 (e176e1714 2025-07-30)

### Python version

Python 3.10.11

---

_Label `bug` added by @sanzenwin on 2025-08-04 23:29_

---

_Comment by @charliermarsh on 2025-08-04 23:32_

I think this would be solved by #14913, but we technically don't support Python 3.7.

---

_Comment by @sanzenwin on 2025-08-06 04:14_

Same errors with uv 0.8.5 (ce3728681 2025-08-05).

A reproducible code sample for Python 3.8: [Github action](https://github.com/sanzenwin/uv-tests/actions/runs/16767324462)

---
