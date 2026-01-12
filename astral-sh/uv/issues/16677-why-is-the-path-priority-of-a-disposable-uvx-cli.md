```yaml
number: 16677
title: why is the path priority of a disposable uvx cli lower than that of a cli with the same name in the windows path environment?
type: issue
state: closed
author: candrapersada
labels:
  - question
assignees: []
created_at: 2025-11-11T00:27:41Z
updated_at: 2025-11-11T12:47:46Z
url: https://github.com/astral-sh/uv/issues/16677
synced_at: 2026-01-12T16:02:36Z
```

# why is the path priority of a disposable uvx cli lower than that of a cli with the same name in the windows path environment?

---

_@candrapersada_

### Question

why is the path priority of a disposable `uvx` cli lower than that of a cli with the same name in the windows path environment?
```
uvx --from "git+https://github.com/yt-dlp/yt-dlp[default]" where yt-dlp
C:\Users\username\bin\yt-dlp.exe
C:\Users\username\AppData\Local\uv\cache\archive-v0\gLYNIqEvZZOr3DvEooNxK\Scripts\yt-dlp.exe
```

### Platform

Windows 10 x86_64

### Version

uv 0.8.9

---

_Label `question` added by @candrapersada on 2025-11-11 00:27_

---

_Closed by @candrapersada on 2025-11-11 12:47_

---
