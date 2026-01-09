---
number: 9660
title: SIM108 autofix changes quote style of nested f-string
type: issue
state: open
author: Gjgarr
labels: []
assignees: []
created_at: 2024-01-28T08:53:54Z
updated_at: 2025-01-28T17:10:16Z
url: https://github.com/astral-sh/ruff/issues/9660
synced_at: 2026-01-07T13:12:15-06:00
---

# SIM108 autofix changes quote style of nested f-string

---

_Issue opened by @Gjgarr on 2024-01-28 08:53_

# Description
SIM108 auto-fix changes quote style of nested f-string unnecessarily.

# Versions
Python version: 3.12
Ruff version: v0.1.14


# Reproduce

Playground link: https://play.ruff.rs/1519e519-94fc-412f-abbb-c8c838a698e7

Given the following:
```py
if 1:
    a = f"# {"".join([])}"
else: 
    a = ""
```

SIM108 autofix converts this to:
```py
a = f"# {''.join([])}" if 1 else ""
```

Expected:
```py
a = f"# {"".join([])}" if 1 else ""
```

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-01-28 11:43_

---

_Comment by @AlexWaygood on 2024-01-28 11:45_

This is a symptom of a more general problem with the way our "expression generator" for creating autofixes works — it often doesn't preserve stylistic details such as quoting style etc. (#7799)

*However*, in this specific case, I think it should be pretty easy to preserve more stylistic details for this rule

---

_Referenced in [astral-sh/ruff#9662](../../astral-sh/ruff/pulls/9662.md) on 2024-01-28 13:38_

---

_Referenced in [astral-sh/ruff#15726](../../astral-sh/ruff/pulls/15726.md) on 2025-01-24 17:14_

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-01-25 18:53_

---

_Assigned to @ntBre by @ntBre on 2025-01-28 17:10_

---

_Referenced in [astral-sh/ruff#15794](../../astral-sh/ruff/pulls/15794.md) on 2025-01-28 21:11_

---
