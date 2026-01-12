```yaml
number: 15265
title: "Abnormal behavior of the `uv python dir` in Windows systems"
type: issue
state: open
author: shaonianche
labels:
  - bug
assignees: []
created_at: 2025-08-14T00:14:03Z
updated_at: 2025-08-14T00:29:03Z
url: https://github.com/astral-sh/uv/issues/15265
synced_at: 2026-01-12T16:02:07Z
```

# Abnormal behavior of the `uv python dir` in Windows systems

---

_@shaonianche_

### Summary

When I redirect the output of `uv python dir` to a file, the resulting text contains ANSI color codes. This makes the output difficult to parse or use in non-terminal environments. The expected behavior is that the command should automatically detect it's not being run in a TTY and output plain text.

<img width="1519" height="314" alt="Image" src="https://github.com/user-attachments/assets/605ebe3f-5eee-4d96-8ada-5e412f286d38" />

### Platform

Windows 11 x86_64, PowerShell 5.1

### Version

uv 0.8.4 (e176e1714 2025-07-30)

### Python version

Python 3.13.6

---

_Label `bug` added by @shaonianche on 2025-08-14 00:14_

---

_Comment by @zanieb on 2025-08-14 00:23_

Just `uv python dir`?

---

_Comment by @shaonianche on 2025-08-14 00:29_

> Just `uv python dir`?

Currently, I have only used `uv python dir`. I will try other commands



---
