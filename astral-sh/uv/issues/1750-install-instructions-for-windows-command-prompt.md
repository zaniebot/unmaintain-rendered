```yaml
number: 1750
title: Install instructions for Windows command prompt
type: issue
state: closed
author: 12932
labels:
  - documentation
  - windows
assignees: []
created_at: 2024-02-20T12:37:17Z
updated_at: 2024-02-22T09:25:34Z
url: https://github.com/astral-sh/uv/issues/1750
synced_at: 2026-01-10T05:40:31Z
```

# Install instructions for Windows command prompt

---

_Issue opened by @12932 on 2024-02-20 12:37_

In the README.md [document](https://github.com/astral-sh/uv/blob/main/README.md)

It says "On Windows." , and then provides a Powershell-only compatible command. Running it in my command prompt gave me an error:
```
'irm' is not recognized as an internal or external command,
operable program or batch file.
```

It works fine in Powerhsell. I think the readme should say powershell only for that specific command

---

_Label `documentation` added by @AlexWaygood on 2024-02-20 12:40_

---

_Label `good first issue` added by @AlexWaygood on 2024-02-20 12:40_

---

_Label `windows` added by @AlexWaygood on 2024-02-20 12:40_

---

_Comment by @AlexWaygood on 2024-02-20 12:42_

Agreed. Ideally we'd give a command that worked on both PowerShell and CMD (or at least instructions for how to do things with the CMD shell). But for now, just documenting that this is a PowerShell-specific command would be an improvement. PR welcome!

---

_Comment by @AlexWaygood on 2024-02-20 13:02_

Looks like you documented this as being PowerShell specific in #1751 -- thanks!

I'll leave this open for now, though, as ideally we'd document a CMD command that works as well (or have a unified command that works on both).

---

_Renamed from "Windows instructions for Getting-started don't work in command prompt" to "Install instructions fpr Windows command prompt" by @konstin on 2024-02-20 13:09_

---

_Label `good first issue` removed by @AlexWaygood on 2024-02-20 13:13_

---

_Renamed from "Install instructions fpr Windows command prompt" to "Install instructions for Windows command prompt" by @12932 on 2024-02-20 15:05_

---

_Comment by @konstin on 2024-02-20 17:49_

Ref: https://github.com/axodotdev/cargo-dist/issues/458 

---

_Closed by @konstin on 2024-02-22 09:25_

---
