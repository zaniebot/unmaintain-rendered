---
number: 12264
title: "`UV_TOOL_BIN_DIR` can't set in windows10 with zsh"
type: issue
state: closed
author: kamiertop
labels:
  - bug
assignees: []
created_at: 2025-03-18T08:59:01Z
updated_at: 2025-03-18T12:37:29Z
url: https://github.com/astral-sh/uv/issues/12264
synced_at: 2026-01-07T13:12:18-06:00
---

# `UV_TOOL_BIN_DIR` can't set in windows10 with zsh

---

_Issue opened by @kamiertop on 2025-03-18 08:59_

### Summary

I want to set `UV_TOOL_BIN_DIR`, I use `export UV_TOOL_BIN_DIR=/g/python/tool`, and exec `uv tool dir`,but already show `C:\Users\{username}\AppData\Roaming\uv\tools`

### Platform

Windows10 x86-64

### Version

uv 0.6.7

### Python version

Python 3.12.6

---

_Label `bug` added by @kamiertop on 2025-03-18 08:59_

---

_Comment by @kamiertop on 2025-03-18 12:37_

- I have solved it!🎉
- I read the [code](https://github.com/astral-sh/uv/blob/e0f81f0d4a904d8c743e776d9f8c9ef5b96f769c/crates/uv-dirs/src/lib.rs#L87)
- In Windows, we can't use env like `\g\python\uv` or `G:\python\uv`, **must use `G:\\python\\uv`**
- Also,  I use zsh in Windows10, I set the environment in ~/.zshrc.

---

_Closed by @kamiertop on 2025-03-18 12:37_

---
