---
number: 15797
title: Creation of a Temporary relocatable executable
type: issue
state: open
author: amasin2111
labels:
  - question
assignees: []
created_at: 2025-09-11T20:06:58Z
updated_at: 2025-09-16T19:56:13Z
url: https://github.com/astral-sh/uv/issues/15797
synced_at: 2026-01-07T13:12:19-06:00
---

# Creation of a Temporary relocatable executable

---

_Issue opened by @amasin2111 on 2025-09-11 20:06_

### Question

I wish to know if uv creates a temporary relocatable in the temp folder with signature as .uv.<random_number>.__relocatable.exe

### Platform

Windows

### Version

0.8.15

---

_Label `question` added by @amasin2111 on 2025-09-11 20:07_

---

_Comment by @zanieb on 2025-09-12 12:28_

Yes, this is how replacing a currently running executable is handled on Windows.

---

_Comment by @amasin2111 on 2025-09-16 19:38_

Just follow-up on this, Why is it modifying so many files in the backend?

---

_Comment by @zanieb on 2025-09-16 19:56_

I don't know what you're asking.

---
