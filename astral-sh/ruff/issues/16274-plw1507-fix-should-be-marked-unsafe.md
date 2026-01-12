```yaml
number: 16274
title: PLW1507 fix should be marked unsafe
type: issue
state: closed
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-02-20T13:15:43Z
updated_at: 2025-02-24T17:29:30Z
url: https://github.com/astral-sh/ruff/issues/16274
synced_at: 2026-01-12T15:54:55Z
```

# PLW1507 fix should be marked unsafe

---

_@dscorbett_

### Description

The fix for [`shallow-copy-environ` (PLW1507)](https://docs.astral.sh/ruff/rules/shallow-copy-environ/) changes the program’s behavior (by design) and should therefore be marked unsafe.
```console
$ ruff --version
ruff 0.9.6

$ cat >plw1507.py <<'# EOF'
import copy
import os
os.environ["X"] = "0"
env = copy.copy(os.environ)
os.environ["X"] = "1"
print(env["X"])
# EOF

$ python plw1507.py
1

$ ruff --isolated check --preview --select PLW1507 plw1507.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat plw1507.py
import copy
import os
os.environ["X"] = "0"
env = os.environ.copy()
os.environ["X"] = "1"
print(env["X"])

$ python plw1507.py
0
```

---

_Label `fixes` added by @AlexWaygood on 2025-02-20 13:18_

---

_Comment by @VascoSch92 on 2025-02-23 22:15_

Can i fix that? :)

---

_Assigned to @VascoSch92 by @ntBre on 2025-02-24 00:06_

---

_Comment by @VascoSch92 on 2025-02-24 17:28_

I think this is closed

---

_Closed by @MichaReiser on 2025-02-24 17:29_

---
