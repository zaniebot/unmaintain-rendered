---
number: 11407
title: Installing x64 Python on aarch64?
type: issue
state: closed
author: tyteen4a03
labels:
  - question
assignees: []
created_at: 2025-02-10T21:41:52Z
updated_at: 2025-02-11T00:08:08Z
url: https://github.com/astral-sh/uv/issues/11407
synced_at: 2026-01-10T01:25:05Z
---

# Installing x64 Python on aarch64?

---

_Issue opened by @tyteen4a03 on 2025-02-10 21:41_

### Question

How do I install a x64 version of Python on an aarch64 M-Chip machine?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @tyteen4a03 on 2025-02-10 21:41_

---

_Comment by @zanieb on 2025-02-10 21:49_

e.g., `uv python install cpython-3.10.16-macos-x86_64-none`

---

_Comment by @tyteen4a03 on 2025-02-11 00:08_

Got it, turns out I needed `--all-archs` for it to show up.

---

_Closed by @tyteen4a03 on 2025-02-11 00:08_

---
