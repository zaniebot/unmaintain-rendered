```yaml
number: 15796
title: C417 false positive when not exactly 1 lambda parameter
type: issue
state: closed
author: dscorbett
labels: []
assignees: []
created_at: 2025-01-28T22:00:28Z
updated_at: 2025-01-29T18:45:56Z
url: https://github.com/astral-sh/ruff/issues/15796
synced_at: 2026-01-10T11:09:57Z
```

# C417 false positive when not exactly 1 lambda parameter

---

_Issue opened by @dscorbett on 2025-01-28 22:00_

### Description

[`unnecessary-map` (C417)](https://docs.astral.sh/ruff/rules/unnecessary-map/) has a false positive in Ruff 0.9.3 when there is not exactly one lambda parameter. If the fix is enabled, it changes the program’s behavior by suppressing or changing the raised error.

```console
$ cat >c417.py <<'# EOF'
try:
    print(list(map(lambda: 1, "xyz")))
except Exception as e:
    print(repr(e))
try:
    print(list(map(lambda x, y: x, [(1, 2), (3, 4)])))
except Exception as e:
    print(repr(e))
try:
    print(list(map(lambda x, y: x, "xyz")))
except Exception as e:
    print(repr(e))
# EOF

$ python c417.py
TypeError('<lambda>() takes 0 positional arguments but 1 was given')
TypeError("<lambda>() missing 1 required positional argument: 'y'")
TypeError("<lambda>() missing 1 required positional argument: 'y'")

$ ruff --isolated check  --select C417 c417.py --unsafe-fixes --fix
Found 3 errors (3 fixed, 0 remaining).

$ cat c417.py
try:
    print([1 for _ in "xyz"])
except Exception as e:
    print(repr(e))
try:
    print([x for x, y in [(1, 2), (3, 4)]])
except Exception as e:
    print(repr(e))
try:
    print([x for x, y in "xyz"])
except Exception as e:
    print(repr(e))

$ python c417.py
[1, 1, 1]
[1, 3]
ValueError('not enough values to unpack (expected 2, got 1)')
```

---

_Closed by @AlexWaygood on 2025-01-29 18:45_

---
