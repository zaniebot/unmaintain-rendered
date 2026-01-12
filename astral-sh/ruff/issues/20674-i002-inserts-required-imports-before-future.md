```yaml
number: 20674
title: I002 inserts required imports before future imports
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-01T20:09:09Z
updated_at: 2025-10-06T13:40:38Z
url: https://github.com/astral-sh/ruff/issues/20674
synced_at: 2026-01-12T15:54:57Z
```

# I002 inserts required imports before future imports

---

_@dscorbett_

### Summary

The fix for [`missing-required-import` (I002)](https://docs.astral.sh/ruff/rules/missing-required-import/) inserts required imports before future imports, which introduces a syntax error. [Example](https://play.ruff.rs/1ea00554-6288-47fa-a3d2-b2c71ad067d1):
```console
$ cat >i002.py <<'# EOF'
from __future__ import annotations
# EOF

$ ruff --isolated check i002.py --select I002 --unsafe-fixes --fix --config 'lint.isort.required-imports = ["import this"]'
Found 1 error (1 fixed, 0 remaining).

$ cat i002.py
import this
from __future__ import annotations

$ python i002.py 2>&1 | tail -n 1
SyntaxError: from __future__ imports must occur at the beginning of the file
```

### Version

ruff 0.13.2 (b0bdf0334 2025-09-25)

---

_Label `bug` added by @ntBre on 2025-10-01 21:56_

---

_Label `fixes` added by @ntBre on 2025-10-01 21:56_

---

_Closed by @ntBre on 2025-10-06 13:40_

---
