```yaml
number: 20694
title: B905 and B912 fixes should be unsafe
type: issue
state: closed
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-10-03T17:32:58Z
updated_at: 2025-10-07T20:55:57Z
url: https://github.com/astral-sh/ruff/issues/20694
synced_at: 2026-01-10T11:09:59Z
```

# B905 and B912 fixes should be unsafe

---

_Issue opened by @dscorbett on 2025-10-03 17:32_

### Summary

The fixes for [`zip-without-explicit-strict` (B905)](https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/) and [`map-without-explicit-strict` (B912)](https://docs.astral.sh/ruff/rules/map-without-explicit-strict/) should be marked unsafe by the same reasoning as in [this comment](https://github.com/astral-sh/ruff/pull/19518#issuecomment-3129685094):
> The issue isn't that our fix would change behavior but that we could silently preserve the wrong behavior. Instead we should alert the user to the issue so that they can decide what it's supposed to say.

[Example](https://play.ruff.rs/827c9cbe-186f-4c52-9a15-71f571b8192d):
```console
$ cat >b9.py <<'# EOF'
print(list(zip("ab", "123")))
print(list(map(lambda x, y: y + x, "ab", "123")))
# EOF

$ python b9.py
[('a', '1'), ('b', '2')]
['1a', '2b']

$ ruff --isolated check b9.py --select B905,B912 --preview --target-version py314 --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat b9.py
print(list(zip("ab", "123", strict=False)))
print(list(map(lambda x, y: y + x, "ab", "123", strict=False)))

$ python3.14 b9.py
[('a', '1'), ('b', '2')]
['1a', '2b']
```

### Version

ruff 0.13.3 (188c0dce2 2025-10-02)

---

_Label `fixes` added by @ntBre on 2025-10-03 19:13_

---

_Closed by @ntBre on 2025-10-07 20:55_

---
