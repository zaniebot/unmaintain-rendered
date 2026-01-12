```yaml
number: 6718
title: "Failed to create fix for SysExitAlias: Unable to modify existing import statement"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-21T08:59:33Z
updated_at: 2023-08-21T23:31:31Z
url: https://github.com/astral-sh/ruff/issues/6718
synced_at: 2026-01-12T15:54:46Z
```

# Failed to create fix for SysExitAlias: Unable to modify existing import statement

---

_@qarmin_

Ruff 0.0.285

```
ruff  *.py --select ALL
```

file content:
```
from sys import *

exit(0)
```

error:
```
error: Failed to create fix for SysExitAlias: Unable to modify existing import statement
```


[PY_FILE_TEST_8681904433.py.zip](https://github.com/astral-sh/ruff/files/12394254/PY_FILE_TEST_8681904433.py.zip)



---

_Label `bug` added by @charliermarsh on 2023-08-21 12:57_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-21 12:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-21 21:00_

---

_Closed by @charliermarsh on 2023-08-21 23:31_

---
