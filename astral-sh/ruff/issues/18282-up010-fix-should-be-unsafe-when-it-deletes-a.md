```yaml
number: 18282
title: UP010 fix should be unsafe when it deletes a comment
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-05-23T18:04:20Z
updated_at: 2025-05-25T10:44:22Z
url: https://github.com/astral-sh/ruff/issues/18282
synced_at: 2026-01-12T15:54:56Z
```

# UP010 fix should be unsafe when it deletes a comment

---

_@dscorbett_

### Summary

The fix for [`unnecessary-future-import` (UP010)](https://docs.astral.sh/ruff/rules/unnecessary-future-import/) should be marked unsafe when it deletes a comment.
```console
$ cat >up010.py <<'# EOF'
from __future__ import print_function  # ...
print()
# EOF

$ ruff --isolated check up010.py --select UP010 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat up010.py 
print()
```

### Version

ruff 0.11.11 (0397682f1 2025-05-22)

---

_Label `bug` added by @MichaReiser on 2025-05-23 18:05_

---

_Label `fixes` added by @MichaReiser on 2025-05-23 18:05_

---

_Label `help wanted` added by @MichaReiser on 2025-05-23 18:05_

---

_Comment by @chirizxc on 2025-05-24 17:39_

can i work on this?

---

_Closed by @MichaReiser on 2025-05-25 10:44_

---
