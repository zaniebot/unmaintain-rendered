---
number: 14799
title: "`uv --version` display different version comparing to `brew list --versions uv`"
type: issue
state: closed
author: ziminz19
labels:
  - question
assignees: []
created_at: 2025-07-21T20:03:26Z
updated_at: 2025-07-21T20:12:46Z
url: https://github.com/astral-sh/uv/issues/14799
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv --version` display different version comparing to `brew list --versions uv`

---

_Issue opened by @ziminz19 on 2025-07-21 20:03_

### Question

I installed uv on macos via `brew install uv`. However, the version displayed by `brew list --versions uv` and `uv --version` is different. `uv --version` always display `uv 0.7.8 (0ddcc1905 2025-05-23)` no matter what is the actual version installed by brew, but `brew list --versions uv` will display the actual version I'm intended to install. Is this a problem with brew or uv? Which version number should I trust?

<img width="321" height="178" alt="Image" src="https://github.com/user-attachments/assets/ce6bf794-66a7-461a-8d10-aec60e70597c" />

### Platform

macOS 15.5

### Version

uv 0.8.0

---

_Label `question` added by @ziminz19 on 2025-07-21 20:03_

---

_Comment by @zanieb on 2025-07-21 20:04_

How do you know you're getting the same uv? What's `where uv`?

---

_Comment by @ziminz19 on 2025-07-21 20:12_

> How do you know you're getting the same uv? What's `where uv`?

Thanks for helping! The issue is resolved. I thought I delete the previously installed uv but apparently I didn't (silly mistake).  Sorry for taking your time, closing the issue.

---

_Closed by @ziminz19 on 2025-07-21 20:12_

---
