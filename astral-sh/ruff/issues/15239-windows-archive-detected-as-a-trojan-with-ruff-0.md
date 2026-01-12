```yaml
number: 15239
title: Windows archive detected as a trojan with ruff 0.8.5
type: issue
state: closed
author: jcwillox
labels:
  - windows
assignees: []
created_at: 2025-01-03T15:03:50Z
updated_at: 2025-01-05T11:08:16Z
url: https://github.com/astral-sh/ruff/issues/15239
synced_at: 2026-01-12T15:54:54Z
```

# Windows archive detected as a trojan with ruff 0.8.5

---

_@jcwillox_

The windows download for 0.8.5, is detected as a trojan, thought I should report it, probably a false positive, but didn't see any other issues for it.

**File**: ruff-x86_64-pc-windows-msvc.zip
**Version**: 0.8.5

This is on Windows 10 with Windows Defender.

![Image](https://github.com/user-attachments/assets/d1048ffc-ea70-4d24-8fe9-929e68ba8330)

Let me know if you need anything else.

---

_Renamed from "Windows Archive Detected as a Trojan with ruff 0.8.5" to "Windows archive detected as a trojan with ruff 0.8.5" by @jcwillox on 2025-01-03 15:04_

---

_Label `windows` added by @AlexWaygood on 2025-01-03 17:38_

---

_Comment by @MichaReiser on 2025-01-05 08:52_

Thanks for reporting. We've historically not been able to do much about this. It helps if users report the warnings as false positives if their anti-virus software supports them.

I upload this specific file to [virustotal](https://www.virustotal.com/gui/file/4ca646f3659a88eb64b73bcfd4a4927c0194005c5aff69f4c71406d4370360c2) and not a single scanner reports it.

For reference:

* https://github.com/astral-sh/ruff/issues/9056
* https://github.com/astral-sh/ruff/issues/12431



---

_Comment by @jcwillox on 2025-01-05 11:08_

Yeah, thought I'd just report it incase it wasn't just a false positive seeing as there didn't appear to be any other issues about it, I was searching for "windows" and "trojan" instead of "virus" (guess this issue should help the search results at least). But anyways like magic, today I was able to download the archive without windows defender complaining and since also updated to 0.8.6 fine.

So who knows what was going on in windows defenders mind 👀


---

_Closed by @jcwillox on 2025-01-05 11:08_

---
