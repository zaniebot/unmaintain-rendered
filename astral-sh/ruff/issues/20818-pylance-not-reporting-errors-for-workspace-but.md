```yaml
number: 20818
title: PyLance not reporting errors for workspace but only reporting for open files
type: issue
state: closed
author: ScarletMcLearn
labels:
  - needs-info
assignees: []
created_at: 2025-10-12T07:30:56Z
updated_at: 2025-10-13T08:54:07Z
url: https://github.com/astral-sh/ruff/issues/20818
synced_at: 2026-01-12T15:54:57Z
```

# PyLance not reporting errors for workspace but only reporting for open files

---

_@ScarletMcLearn_

### Summary

**Environment data**

- Pylance version: 2025.8.3
- OS and version: Windows 11 (latest)
- Python version: 3.11

**Repro Steps**

- Have the following ruff.toml, ruff(2).toml and settings.json : https://gist.github.com/ScarletMcLearn/64d2596e0211ff085856e1d5945b5a07
- Note that diagnosis is set for workspace

**Expected behavior**

- The errors should show for workspace

**Actual behavior**

- No logs showing for workspace - only shows for open files
- It was working before I created the backup toml file
- Ever since I created it - the ruff(2).toml and stored my old one in it and added new one to ruff.toml it stopped working

**Notes:**

- I have tried restarting language server, vscode, pc - but no fix.

### Version

ruff 0.13.2 (b0bdf0334 2025-09-25)

---

_Comment by @MichaReiser on 2025-10-12 17:15_

Hi. 

Can you tell me more about what diagnostics you aren't seeing. Is it Ruff diagnostics or Pylance diagnostics? 

Ruff doesn't support workspace diagnostics. It only shows diagnostics for the open files. If it's something about Pylance, then I'm sorry, but you'll have to ask on the Pylance issue tracker.

---

_Label `needs-info` added by @MichaReiser on 2025-10-12 17:15_

---

_Comment by @ScarletMcLearn on 2025-10-13 08:34_

I am talking about the warnings, errors and info in the PROBLEMS tab in the bottom section.

---

_Comment by @MichaReiser on 2025-10-13 08:52_

> I am talking about the warnings, errors and info in the PROBLEMS tab in the bottom section.

I understand that. But those warnings and errors can come from different extensions. E.g. some warnings come from Ruff where others come from Pylance. 

Ruff doesn't support workspace diagnostics. So the behavior you're seeing is expected. What's up with Pylance, I can't say. You'd have to ask the pylance maintainers for help

---

_Comment by @ScarletMcLearn on 2025-10-13 08:54_

Thanks! Clarified!

---

_Closed by @ScarletMcLearn on 2025-10-13 08:54_

---
