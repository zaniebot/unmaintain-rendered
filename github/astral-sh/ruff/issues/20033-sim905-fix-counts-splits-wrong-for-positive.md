---
number: 20033
title: "SIM905 fix counts splits wrong for positive `maxsplit` and unspecified `sep`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-08-21T20:21:49Z
updated_at: 2025-09-18T20:56:36Z
url: https://github.com/astral-sh/ruff/issues/20033
synced_at: 2026-01-07T13:12:16-06:00
---

# SIM905 fix counts splits wrong for positive `maxsplit` and unspecified `sep`

---

_Issue opened by @dscorbett on 2025-08-21 20:21_

### Summary

The fix for [`split-static-string` (SIM905)](https://docs.astral.sh/ruff/rules/split-static-string/) with positive `maxsplit` and unspecified `sep` counts the splits on individual white space characters. This can change the program’s behavior. It should count the splits on white space sequences. [Example](https://play.ruff.rs/273894ac-88a5-4c19-8285-68adcf6f77c2):
```console
$ cat >sim905.py <<'# EOF'
print("a  b".split(maxsplit=1))
# EOF

$ python sim905.py
['a', 'b']

$ ruff --isolated check sim905.py --select SIM905 --preview --fix
Found 1 error (1 fixed, 0 remaining).

$ python sim905.py
['a', ' b']
```

### Version

ruff 0.12.10 (c68ff8d90 2025-08-21)

---

_Label `bug` added by @ntBre on 2025-08-21 23:58_

---

_Label `fixes` added by @ntBre on 2025-08-21 23:58_

---

_Referenced in [astral-sh/ruff#20056](../../astral-sh/ruff/pulls/20056.md) on 2025-08-23 15:03_

---

_Closed by @ntBre on 2025-09-18 20:56_

---
