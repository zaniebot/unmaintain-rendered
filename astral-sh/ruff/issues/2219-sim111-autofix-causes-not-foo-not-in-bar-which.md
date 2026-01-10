```yaml
number: 2219
title: "SIM111 autofix causes \"not foo not in bar\" which can be enhanced"
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-26T20:28:15Z
updated_at: 2023-02-12T22:39:31Z
url: https://github.com/astral-sh/ruff/issues/2219
synced_at: 2026-01-10T11:09:45Z
```

# SIM111 autofix causes "not foo not in bar" which can be enhanced

---

_Issue opened by @spaceone on 2023-01-26 20:28_

`SIM111` simplifies 
```diff
-               for oc in self.objectClasses:
-                       if oc not in objectClasses:
-                               return False
-               return True
+               return all(not oc not in objectClasses for oc in self.objectClasses)
```

better would be:
`return not any(oc in objectClasses for oc in self.objectClasses)`

---

_Renamed from "SIM111 enhancement" to "SIM111 autofix causes "not foo not in bar" which can be enhanced" by @spaceone on 2023-01-30 12:23_

---

_Comment by @spaceone on 2023-01-31 23:00_

could be identified and fixed by `SIM110` instead of `SIM111` then.

---

_Closed by @charliermarsh on 2023-02-12 22:39_

---
