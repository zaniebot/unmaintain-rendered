---
number: 20198
title: "C417 fix introduces a syntax error for `yield` in lambda expression"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-09-01T19:28:22Z
updated_at: 2025-09-03T20:39:12Z
url: https://github.com/astral-sh/ruff/issues/20198
synced_at: 2026-01-10T01:23:01Z
---

# C417 fix introduces a syntax error for `yield` in lambda expression

---

_Issue opened by @dscorbett on 2025-09-01 19:28_

### Summary

The fix for [`unnecessary-map` (C417)](https://docs.astral.sh/ruff/rules/unnecessary-map/) introduces a syntax error when the lambda expression contains a yield expression. The fix, and possibly the rule, should be suppressed in that case. [Example](https://play.ruff.rs/dffb7894-a98f-4643-9efd-67ad02563071):
```console
$ cat >c417.py <<'# EOF'
base = 0
gens = map(lambda x: (yield base + x), [1, 2, 3])
for gen in gens:
    print(*[*gen])
    base += 20
# EOF

$ python c417.py
1
22
43

$ ruff --isolated check c417.py --select C417 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ python c417.py 2>&1 | tail -n 3
    gens = ((yield base + x) for x in [1, 2, 3])
             ^^^^^^^^^^^^^^
SyntaxError: 'yield' inside generator expression
```

### Version

ruff 0.12.11 (c2bc15bc1 2025-08-28)

---

_Referenced in [astral-sh/ruff#20201](../../astral-sh/ruff/pulls/20201.md) on 2025-09-02 01:18_

---

_Label `bug` added by @ntBre on 2025-09-02 02:49_

---

_Label `fixes` added by @ntBre on 2025-09-02 02:49_

---

_Closed by @ntBre on 2025-09-03 20:39_

---
