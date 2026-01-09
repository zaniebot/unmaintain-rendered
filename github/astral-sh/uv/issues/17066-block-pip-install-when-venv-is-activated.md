---
number: 17066
title: Block pip install when venv is activated
type: issue
state: closed
author: Stefanhg
labels:
  - enhancement
assignees: []
created_at: 2025-12-10T11:07:03Z
updated_at: 2025-12-10T13:29:54Z
url: https://github.com/astral-sh/uv/issues/17066
synced_at: 2026-01-07T13:12:19-06:00
---

# Block pip install when venv is activated

---

_Issue opened by @Stefanhg on 2025-12-10 11:07_

### Summary

Hello,

When you activate the venv in UV, pip install is still available in the global installation, meaning if you do `pip install pytest`, then pytest is installed globally rather than within UV, since UV does not have a pip.exe within it's venv.

I would like UV to block or redirect directly executing `pip install` when the venv is activated 
Highly preferred; Redirect `pip install` to `UV pip` for backward compatibility.

### Example

```
uv init
uv sync
.venv\Scripts\Activate
pip install pytest
uv pip list 
```
Does not include pytest - Expected.

Expected behaviour:
```
venv\Scripts\Activate
pip install pytest
Exception("Pip cannot be accessed directly, use uv pip install ..")
```

Or ideally, redirect command to uv pip.

---

_Label `enhancement` added by @Stefanhg on 2025-12-10 11:07_

---

_Comment by @zanieb on 2025-12-10 13:16_

This is a duplicate of https://github.com/astral-sh/uv/issues/12604

---

_Comment by @Stefanhg on 2025-12-10 13:29_

@zanieb I was trying to find related issue, thank you!
CLosed because of duplication. 

---

_Closed by @Stefanhg on 2025-12-10 13:29_

---
