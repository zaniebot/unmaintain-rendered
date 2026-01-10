```yaml
number: 21209
title: RUF057 has false positives with starred arguments
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-11-02T22:28:59Z
updated_at: 2025-11-05T17:07:34Z
url: https://github.com/astral-sh/ruff/issues/21209
synced_at: 2026-01-10T11:10:00Z
```

# RUF057 has false positives with starred arguments

---

_Issue opened by @dscorbett on 2025-11-02 22:28_

### Summary

[`unnecessary-round` (RUF057)](https://docs.astral.sh/ruff/rules/unnecessary-round/) has false positives with starred arguments. [Example](https://play.ruff.rs/2269acbd-1a7c-4086-8e28-0d89d6c20907):
```console
$ cat >ruf057.py <<'# EOF'
print(round(125, **{"ndigits": -2}))
print(round(125, *[-2]))
# EOF

$ python ruf057.py
100
100

$ ruff --isolated check ruf057.py --select RUF057 --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat ruf057.py
print((125))
print((125))

$ python ruf057.py
125
125
```

### Version

ruff 0.14.3 (8737a2d5f 2025-10-30)

---

_Label `bug` added by @ntBre on 2025-11-03 14:53_

---

_Label `rule` added by @ntBre on 2025-11-03 14:53_

---

_Closed by @ntBre on 2025-11-05 17:07_

---
