```yaml
number: 21985
title: There is an issue on Windows 7 x64.
type: issue
state: closed
author: pengfan806
labels:
  - question
assignees: []
created_at: 2025-12-15T09:21:22Z
updated_at: 2025-12-15T14:54:08Z
url: https://github.com/astral-sh/ruff/issues/21985
synced_at: 2026-01-12T15:54:58Z
```

# There is an issue on Windows 7 x64.

---

_@pengfan806_

### Summary

Run ruff.exe popup a error message:

[ruff.exe - Entrance cannot be found]
Unable to locate the procedure entry point GetSystemTimePreciseAsFileTime in the dynamic link library kernel32.dll.

### Version

_No response_

---

_Comment by @MichaReiser on 2025-12-15 09:24_

Hi @pengfan806 

I'm sorry, but we no longer build Windows 7-compatible binaries because Rust stopped supporting Windows 7.

---

_Label `question` added by @MichaReiser on 2025-12-15 09:24_

---

_Comment by @pengfan806 on 2025-12-15 09:29_

ok, clear

---

_Closed by @pengfan806 on 2025-12-15 09:29_

---

_Comment by @MichaReiser on 2025-12-15 10:26_

You could try if an older ruff version works (released sometime around Feb 2024)

---

_Comment by @pengfan806 on 2025-12-15 14:54_

I downloaded a 64-bit version of Ruff from February 2024, and it works fine on 64-bit Windows 7. Thank you!

---
