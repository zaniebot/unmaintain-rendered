```yaml
number: 20151
title: G004 fix can change behavior and introduce syntax errors
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
  - preview
assignees: []
created_at: 2025-08-29T12:59:09Z
updated_at: 2025-12-01T21:51:32Z
url: https://github.com/astral-sh/ruff/issues/20151
synced_at: 2026-01-12T15:54:57Z
```

# G004 fix can change behavior and introduce syntax errors

---

_@dscorbett_

### Summary

The fix for [`logging-f-string` (G004)](https://docs.astral.sh/ruff/rules/logging-f-string/) transforms the argument incorrectly in various ways, changing the program’s behavior. [Example](https://play.ruff.rs/14641467-b12d-4186-bfd0-ca4dc1f99fcd):
```console
$ cat >g004_1.py <<'# EOF'
import logging
logging.warning(fr"\'{Ellipsis}")
logging.warning(f"{Ellipsis}" "!")
logging.warning((f"{Ellipsis}"))
logging.warning(f"\x5c{Ellipsis}")
# EOF

$ python g004_1.py
WARNING:root:\'Ellipsis
WARNING:root:Ellipsis!
WARNING:root:Ellipsis
WARNING:root:\Ellipsis

$ ruff --isolated check g004_1.py --select G004 --preview --fix
Found 4 errors (4 fixed, 0 remaining).

$ cat g004_1.py
import logging
logging.warning("\'%s", Ellipsis)
logging.warning("%s", Ellipsis)
logging.warning(("%s", Ellipsis))
logging.warning("\%s", Ellipsis)

$ python g004_1.py
g004_1.py:5: SyntaxWarning: invalid escape sequence '\%'
  logging.warning("\%s", Ellipsis)
WARNING:root:'Ellipsis
WARNING:root:Ellipsis
WARNING:root:('%s', Ellipsis)
WARNING:root:\Ellipsis
```

It can also introduce syntax errors. [Example](https://play.ruff.rs/4b8cff4f-804f-47f7-a0ef-7213fb4a9e2b):
```console
$ cat >g004_2.py <<'# EOF'
import logging
logging.warning(f"\n{Ellipsis}")
logging.warning(f"\x22{Ellipsis}")
# EOF

$ python g004_2.py
WARNING:root:
Ellipsis
WARNING:root:"Ellipsis

$ ruff --isolated check g004_2.py --select G004 --preview --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.12.11 (c2bc15bc1 2025-08-28)

---

_Label `bug` added by @ntBre on 2025-08-29 13:08_

---

_Label `fixes` added by @ntBre on 2025-08-29 13:08_

---

_Label `preview` added by @ntBre on 2025-08-29 13:08_

---

_Comment by @TaKO8Ki on 2025-09-03 15:07_

I’ll take this one next.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-09-03 15:07_

---

_Comment by @ericrbg-harmonic on 2025-12-01 21:51_

another one is something like:
```
x = 1
logging.info(f"{x=}")
```

will go to

```
logging.info("%s", x)
```

---
