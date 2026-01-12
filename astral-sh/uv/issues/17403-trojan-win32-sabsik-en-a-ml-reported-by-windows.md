```yaml
number: 17403
title: "Trojan: Win32/Sabsik.EN.A!ml reported by Windows Security while updating to version 0.9.24"
type: issue
state: open
author: zoliszabo
labels:
  - question
assignees: []
created_at: 2026-01-10T20:11:09Z
updated_at: 2026-01-12T02:25:03Z
url: https://github.com/astral-sh/uv/issues/17403
synced_at: 2026-01-12T16:02:50Z
```

# Trojan: Win32/Sabsik.EN.A!ml reported by Windows Security while updating to version 0.9.24

---

_@zoliszabo_

### Summary

<img width="511" height="426" alt="Image" src="https://github.com/user-attachments/assets/966b3815-8972-4191-a818-eab81b3dc181" />

Tried to update to latest version (using `winget`) and Windows Security kicked in. See screenshot for reported problem.

### Platform

Windows 11 Pro, 25H2, x64

### Version

0.9.24

### Python version

_No response_

---

_Label `bug` added by @zoliszabo on 2026-01-10 20:11_

---

_Renamed from "Trojan: Win32/Sabsik.EN.A!ml reported by Windows Security whily updating to version 0.9.24" to "Trojan: Win32/Sabsik.EN.A!ml reported by Windows Security while updating to version 0.9.24" by @zoliszabo on 2026-01-11 10:24_

---

_Comment by @zanieb on 2026-01-12 02:24_

There is no virus here, this is just noise from Windows Security. The `!ml` suffix indicates this is a machine learning based heuristic. This happens often for new releases until their heuristics adjust, e.g., see https://github.com/astral-sh/uv/issues/17344

---

_Label `bug` removed by @zanieb on 2026-01-12 02:25_

---

_Label `question` added by @zanieb on 2026-01-12 02:25_

---
