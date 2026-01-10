```yaml
number: 21315
title: "RUF065 should ignore `str` when not used for conversion"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
  - preview
assignees: []
created_at: 2025-11-07T14:02:06Z
updated_at: 2025-11-10T23:04:42Z
url: https://github.com/astral-sh/ruff/issues/21315
synced_at: 2026-01-10T11:10:00Z
```

# RUF065 should ignore `str` when not used for conversion

---

_Issue opened by @dscorbett on 2025-11-07 14:02_

### Summary

[`logging-eager-conversion` (RUF065)](https://docs.astral.sh/ruff/rules/logging-eager-conversion/) should only treat `str` as an unnecessary conversion when it has exactly one argument and that argument isn’t starred. Otherwise, the `str` call is doing something else and can’t be so straightforwardly removed. [Example](https://play.ruff.rs/d4bd231e-6994-449a-9639-b344f7a2ed78):
```console
$ cat >ruf065.py <<'# EOF'
import logging
logging.warning("%s", str())
logging.warning("%s", str(b"\xe2\x9a\xa0", "utf-8"))
logging.warning("%s", str(*(b"\xf0\x9f\x9a\xa7", "utf-8")))
logging.warning("%s", str(**{"object": b"\xf0\x9f\x9a\xa8", "encoding": "utf-8"}))
# EOF

$ ruff --isolated check ruf065.py --select RUF065 --preview --output-format concise -q
ruf065.py:2:23: RUF065 Unnecessary `str()` conversion when formatting with `%s`
ruf065.py:3:23: RUF065 Unnecessary `str()` conversion when formatting with `%s`
ruf065.py:4:23: RUF065 Unnecessary `str()` conversion when formatting with `%s`
ruf065.py:5:23: RUF065 Unnecessary `str()` conversion when formatting with `%s`
```

### Version

ruff 0.14.4 (c7ff9826d 2025-11-06)

---

_Label `bug` added by @ntBre on 2025-11-07 18:59_

---

_Label `rule` added by @ntBre on 2025-11-07 18:59_

---

_Label `preview` added by @ntBre on 2025-11-07 18:59_

---

_Closed by @ntBre on 2025-11-10 23:04_

---
