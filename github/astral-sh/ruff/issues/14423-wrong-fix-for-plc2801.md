---
number: 14423
title: Wrong fix for PLC2801
type: issue
state: closed
author: bebound
labels:
  - bug
assignees: []
created_at: 2024-11-18T03:50:10Z
updated_at: 2024-11-18T04:10:42Z
url: https://github.com/astral-sh/ruff/issues/14423
synced_at: 2026-01-07T13:12:16-06:00
---

# Wrong fix for PLC2801

---

_Issue opened by @bebound on 2024-11-18 03:50_

`ruff check kk.py --select PLC2801 --fix --preview --unsafe-fixes --isolated` change the code from
``` python
if 'kk'.__contains__("k"):
    print("yes")
```
to
``` python
if 'kk' in "k":
    print("yes")
```

The correct fix is `if 'k' in 'kk'`.

Version: ruff 0.7.4

---

_Referenced in [Azure/azure-cli#30365](../../Azure/azure-cli/pulls/30365.md) on 2024-11-18 03:52_

---

_Label `bug` added by @charliermarsh on 2024-11-18 03:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-18 03:52_

---

_Referenced in [astral-sh/ruff#14424](../../astral-sh/ruff/pulls/14424.md) on 2024-11-18 03:53_

---

_Closed by @charliermarsh on 2024-11-18 03:58_

---

_Comment by @bebound on 2024-11-18 04:06_

Woo, that's so fast. Thank you.

---

_Comment by @charliermarsh on 2024-11-18 04:10_

Thanks for pointing that out!

---
