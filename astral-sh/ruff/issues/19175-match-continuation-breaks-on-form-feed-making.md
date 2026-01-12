```yaml
number: 19175
title: "`match_continuation` breaks on form feed, making TC004 introduce a syntax error"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-07-07T11:49:11Z
updated_at: 2025-07-10T14:09:35Z
url: https://github.com/astral-sh/ruff/issues/19175
synced_at: 2026-01-12T15:54:56Z
```

# `match_continuation` breaks on form feed, making TC004 introduce a syntax error

---

_@dscorbett_

### Summary

[`match_continuation`](https://github.com/astral-sh/ruff/blob/0.12.2/crates/ruff_linter/src/importer/insertion.rs#L306) does not recognize U+000C FORM FEED as white space. That makes the fix for [`runtime-import-in-type-checking-block` (TC004)](https://docs.astral.sh/ruff/rules/runtime-import-in-type-checking-block/) ignore a line continuation character after a form feed after a `TYPE_CHECKING` import, introducing a syntax error. [Example](https://play.ruff.rs/f70b43cb-0a49-48ae-b35f-ee85101123f8):
```console
$ cat >tc004.py <<'# EOF'
from typing import TYPE_CHECKING\

if TYPE_CHECKING: import builtins
builtins.print("!")
# EOF

$ ruff --isolated check tc004.py --select TC004 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```
There is a form feed before the backslash in that example. Some browsers hide control characters. Hopefully it’s still there if you copy and paste it.

### Version

ruff 0.12.2 (9bee8376a 2025-07-03)

---

_Label `bug` added by @ntBre on 2025-07-07 20:59_

---

_Label `fixes` added by @ntBre on 2025-07-07 20:59_

---

_Closed by @MichaReiser on 2025-07-10 14:09_

---
