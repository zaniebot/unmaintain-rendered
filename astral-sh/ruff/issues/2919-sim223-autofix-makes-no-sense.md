```yaml
number: 2919
title: SIM223 autofix makes no sense?
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-02-15T12:50:30Z
updated_at: 2023-03-30T15:52:45Z
url: https://github.com/astral-sh/ruff/issues/2919
synced_at: 2026-01-12T15:54:43Z
```

# SIM223 autofix makes no sense?

---

_@spaceone_

`SIM223` fixes:

```diff
-               if False and self._requestid2response_queue:  # FIXME: check for open requests
+               if False:  # FIXME: check for open requests
```

this is temporarily deactivated code.
`if False: …` makes even less sense.
The whole if-block could be removed but I doubt this is good for some autofixer.

---

_Label `autofix` added by @charliermarsh on 2023-02-15 20:46_

---

_Closed by @charliermarsh on 2023-03-30 15:52_

---
