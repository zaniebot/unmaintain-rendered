```yaml
number: 2378
title: "[Panic] if run with --statistics to a file without linter errors"
type: issue
state: closed
author: ded42r
labels:
  - bug
assignees: []
created_at: 2023-01-31T05:37:54Z
updated_at: 2023-01-31T12:53:30Z
url: https://github.com/astral-sh/ruff/issues/2378
synced_at: 2026-01-10T11:09:45Z
```

# [Panic] if run with --statistics to a file without linter errors

---

_Issue opened by @ded42r on 2023-01-31 05:37_

I ran the command `ruff --statistics main.py` and got an error. File _main.py_ is empty.

```
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', ruff_cli\src\printer.rs:379:21
stack backtrace:
   0:     0x7ff7eb613e65 - <unknown>
   1:     0x7ff7eb0715bb - <unknown>
   2:     0x7ff7eb60bbe1 - <unknown>
   3:     0x7ff7eb617012 - <unknown>
   4:     0x7ff7eb616c5d - <unknown>
   5:     0x7ff7eb2e2487 - <unknown>
   6:     0x7ff7eb617980 - <unknown>
   7:     0x7ff7eb6176d3 - <unknown>
   8:     0x7ff7eb61543f - <unknown>
   9:     0x7ff7eb6173b4 - <unknown>
  10:     0x7ff7eb6c4475 - <unknown>
  11:     0x7ff7eb6c431c - <unknown>
  12:     0x7ff7eb2d993d - <unknown>
  13:     0x7ff7eb2e8f47 - <unknown>
  14:     0x7ff7eb2deede - <unknown>
  15:     0x7ff7eb2ea781 - <unknown>
  16:     0x7ff7eb22c576 - <unknown>
  17:     0x7ff7eb60482d - <unknown>
  18:     0x7ff7eb30656c - <unknown>
  19:                0x2 - <unknown>
  20:     0x7ff7eb6c26c1 - <unknown>
  21:     0x7ff7eb6c23cc - <unknown>
  22:     0x7ffb53b67034 - BaseThreadInitThunk
  23:     0x7ffb53e026a1 - RtlUserThreadStart
```

**environment**: ruff 0.0.238, Windows 10

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-31 12:17_

---

_Comment by @charliermarsh on 2023-01-31 12:18_

Thank you! Will fix.

---

_Label `bug` added by @charliermarsh on 2023-01-31 12:38_

---

_Closed by @charliermarsh on 2023-01-31 12:53_

---
