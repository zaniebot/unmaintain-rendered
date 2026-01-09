---
number: 18319
title: "UP015 fix should be unsafe when it removes `\"U\"`"
type: issue
state: closed
author: dscorbett
labels: []
assignees: []
created_at: 2025-05-26T14:15:56Z
updated_at: 2025-05-26T14:43:39Z
url: https://github.com/astral-sh/ruff/issues/18319
synced_at: 2026-01-07T13:12:16-06:00
---

# UP015 fix should be unsafe when it removes `"U"`

---

_Issue opened by @dscorbett on 2025-05-26 14:15_

### Summary

The fix for [`redundant-open-modes` (UP015)](https://docs.astral.sh/ruff/rules/redundant-open-modes/) should be marked unsafe when it removes `"U"` from the mode because doing so changes behavior in Python 3.11 and later.

```console
$ cat >up015.py <<'# EOF'
open("up015.py", "U")
# EOF

$ python3.11 up015.py 2>&1 | tail -n 1
ValueError: invalid mode: 'U'

$ ruff --isolated check up015.py --select UP015 --fix
Found 1 error (1 fixed, 0 remaining).

$ python3.11 up015.py; echo $?
0
```

### Version

ruff 0.11.11 (0397682f1 2025-05-22)

---

_Comment by @MichaReiser on 2025-05-26 14:43_

I think this is fine, given that it fixes an error rather than introduces a new error (e.g. somewhat upgrading from Python 3.3 would find the fix useful rather than annoying). 

---

_Closed by @MichaReiser on 2025-05-26 14:43_

---
