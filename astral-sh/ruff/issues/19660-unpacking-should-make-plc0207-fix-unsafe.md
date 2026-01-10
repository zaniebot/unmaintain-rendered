```yaml
number: 19660
title: "`*` unpacking should make PLC0207 fix unsafe"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-07-31T11:44:44Z
updated_at: 2025-08-06T18:19:50Z
url: https://github.com/astral-sh/ruff/issues/19660
synced_at: 2026-01-10T11:09:59Z
```

# `*` unpacking should make PLC0207 fix unsafe

---

_Issue opened by @dscorbett on 2025-07-31 11:44_

### Summary

The fix for [`missing-maxsplit-arg` (PLC0207)](https://docs.astral.sh/ruff/rules/missing-maxsplit-arg/) should be marked unsafe when the call contains a `*` unpacking, as it already is for a `**` unpacking, because it can induce an error. [Example](https://play.ruff.rs/098e6a46-86e7-42f9-b851-0e026d395e20):
```console
$ cat >plc0207.py <<'# EOF'
"www.example.com".split(".", *[-1])[0]
# EOF

$ ruff --isolated check plc0207.py --select PLC0207 --preview --fix
Found 1 error (1 fixed, 0 remaining).

$ cat plc0207.py
"www.example.com".split(".", *[-1], maxsplit=1)[0]

$ python plc0207.py 2>&1 | tail -n 1
TypeError: split() takes at most 2 arguments (3 given)
```

### Version

ruff 0.12.7 (c5ac99889 2025-07-29)

---

_Label `bug` added by @ntBre on 2025-07-31 12:46_

---

_Label `fixes` added by @ntBre on 2025-07-31 12:46_

---

_Closed by @ntBre on 2025-08-06 18:19_

---
