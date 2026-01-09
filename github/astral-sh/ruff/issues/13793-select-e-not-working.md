---
number: 13793
title: "select = [\"E\"] Not working. "
type: issue
state: closed
author: lozik4
labels:
  - question
assignees: []
created_at: 2024-10-17T13:36:18Z
updated_at: 2024-10-17T14:16:05Z
url: https://github.com/astral-sh/ruff/issues/13793
synced_at: 2026-01-07T13:12:16-06:00
---

# select = ["E"] Not working. 

---

_Issue opened by @lozik4 on 2024-10-17 13:36_

ruff==0.6.9
Python 3.12
MacOS 
Pytest Framework

pyproject.toml
```
[tool.ruff.lint]
select = ["E", "W", "F", "C90", "I", "COM", "ISC", "PT", "Q", "ARG", "PLC", "PLE", "A"]
ignore = ["F403", "F405", "E266", "PT013"]
fixable = ["ALL"]
unfixable = []
preview = true
explicit-preview-rules = true
```

```
ruff check --fix .
All checks passed!
```

But
E302, and E303 are not working, but if I moderate select block:
select = ["E", "W", "F", "C90", "I", "COM", "ISC", "PT", "Q", "ARG", "PLC", "PLE", "A", **"E302", "E303"**]
It helps fix




---

_Comment by @zanieb on 2024-10-17 13:45_

Hi! Those rules are in preview and you have `explicit-preview-rules` turned on which requires exact rule codes for preview rules

- https://docs.astral.sh/ruff/preview/#using-rules-that-are-in-preview
- https://docs.astral.sh/ruff/preview/#selecting-single-preview-rules

---

_Label `question` added by @zanieb on 2024-10-17 13:45_

---

_Comment by @lozik4 on 2024-10-17 14:16_

Thanks

---

_Closed by @lozik4 on 2024-10-17 14:16_

---
