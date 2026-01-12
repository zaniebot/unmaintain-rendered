```yaml
number: 7313
title: Consider suggesting using tempfile classes as context managers 
type: issue
state: closed
author: NeilGirdhar
labels:
  - rule
assignees: []
created_at: 2023-09-12T18:42:09Z
updated_at: 2024-10-08T13:42:01Z
url: https://github.com/astral-sh/ruff/issues/7313
synced_at: 2026-01-12T15:54:47Z
```

# Consider suggesting using tempfile classes as context managers 

---

_@NeilGirdhar_

Just as in:
```
f = Path.open('x')  # SIM115 Use context handler for opening files
f.close()
```
Ruff nudges the user to use context managers, Ruff should do the same for:
```
from tempfile import NamedTemporaryFile
f = NamedTemporaryFile()  # Should nudge SIM115
f.close()
```
and similarly for the other classes in `tempfile`.

---

_Renamed from "Consider suggesting using context managers with tempfile classes" to "Consider suggesting using tempfile classes as context managers " by @NeilGirdhar on 2023-09-12 18:42_

---

_Label `rule` added by @zanieb on 2023-09-12 18:51_

---

_Comment by @zanieb on 2023-09-12 18:52_

Sounds good to me!

---

_Closed by @AlexWaygood on 2024-08-22 13:18_

---

_Comment by @AlexWaygood on 2024-08-27 13:54_

Reopening the issue for now so we remember to stabilise the new behaviour as part of Ruff 0.7

---

_Reopened by @AlexWaygood on 2024-08-27 13:54_

---

_Added to milestone `v0.7` by @AlexWaygood on 2024-08-27 13:55_

---

_Closed by @AlexWaygood on 2024-10-08 13:42_

---
