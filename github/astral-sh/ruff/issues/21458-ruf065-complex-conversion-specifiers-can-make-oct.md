---
number: 21458
title: "RUF065: Complex conversion specifiers can make `oct` and `hex` not unnecessary"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
  - preview
assignees: []
created_at: 2025-11-14T15:39:21Z
updated_at: 2025-11-19T08:38:34Z
url: https://github.com/astral-sh/ruff/issues/21458
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF065: Complex conversion specifiers can make `oct` and `hex` not unnecessary

---

_Issue opened by @dscorbett on 2025-11-14 15:39_

### Summary

[`logging-eager-conversion` (RUF065)](https://docs.astral.sh/ruff/rules/logging-eager-conversion/) calls `oct` and `hex` calls unnecessary with the `s` conversion type, but they are necessary when any of these is true of the conversion specifier:

* The conversion flag `0` is used, the conversion flag `-` is not used, and a minimum width is specified.
* The conversion flag ` ` is used.
* The conversion flag `+` is used.
* A precision is specified.

The string type `s` interprets those components differently than the numeric types `o` and `x`. RUF065’s recommendation leads to a change in behavior, so the calls are not unnecessary, so RUF065 should be suppressed in those cases. [Example](https://play.ruff.rs/f0dd1bae-62ed-40f7-b6ea-4cff4a9e65ce):
```console
$ cat >ruf065.py <<'# EOF'
import logging
logging.warning("%06s", oct(123))
logging.warning("% s", oct(123))
logging.warning("%+s", oct(123))
logging.warning("%.3s", hex(123))
# EOF

$ python ruf065.py
WARNING:root: 0o173
WARNING:root:0o173
WARNING:root:0o173
WARNING:root:0x7

$ ruff --isolated check ruf065.py --select RUF065 --preview --output-format concise -q
ruf065.py:2:25: RUF065 Unnecessary `oct()` conversion when formatting with `%s`. Use `%#o` instead of `%s`
ruf065.py:3:24: RUF065 Unnecessary `oct()` conversion when formatting with `%s`. Use `%#o` instead of `%s`
ruf065.py:4:24: RUF065 Unnecessary `oct()` conversion when formatting with `%s`. Use `%#o` instead of `%s`
ruf065.py:5:25: RUF065 Unnecessary `hex()` conversion when formatting with `%s`. Use `%#x` instead of `%s`

$ sed -E 's/%/%#/; /oct/s/s"/o"/; /hex/s/s"/x"/; s/hex|oct//' ruf065.py | tee ruf065_fixed.py
import logging
logging.warning("%#06o", (123))
logging.warning("%# o", (123))
logging.warning("%#+o", (123))
logging.warning("%#.3x", (123))

$ python ruf065_fixed.py
WARNING:root:0o0173
WARNING:root: 0o173
WARNING:root:+0o173
WARNING:root:0x07b
```

### Version

ruff 0.14.5 (87dafb878 2025-11-13)

---

_Label `bug` added by @ntBre on 2025-11-14 16:26_

---

_Label `rule` added by @ntBre on 2025-11-14 16:26_

---

_Label `preview` added by @ntBre on 2025-11-14 16:26_

---

_Referenced in [astral-sh/ruff#21464](../../astral-sh/ruff/pulls/21464.md) on 2025-11-14 22:47_

---

_Closed by @MichaReiser on 2025-11-19 08:38_

---
