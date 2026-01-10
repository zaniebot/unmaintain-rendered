---
number: 20310
title: ISC rules do not support t-strings
type: issue
state: closed
author: dscorbett
labels:
  - rule
  - python314
assignees: []
created_at: 2025-09-08T21:21:16Z
updated_at: 2025-09-12T18:00:13Z
url: https://github.com/astral-sh/ruff/issues/20310
synced_at: 2026-01-10T01:23:01Z
---

# ISC rules do not support t-strings

---

_Issue opened by @dscorbett on 2025-09-08 21:21_

### Summary

[The ISC rules](https://docs.astral.sh/ruff/rules/#flake8-implicit-str-concat-isc) don’t support t-strings. [Example](https://play.ruff.rs/ec0ae1e6-5942-4f87-9a1f-20a16f734fd4):
```console
$ cat >isc.py <<'# EOF'
# ISC001
t"The quick " t"brown fox."

# ISC002
t"The quick brown fox jumps over the lazy "\
t"dog."

# ISC003
(
    t"The quick brown fox jumps over the lazy "
    + t"dog"
)
# EOF

$ ruff --isolated check isc.py --select ISC --preview --target-version py314
All checks passed!

$ sed 's/t"/"/g' isc.py | ruff --isolated check - --select ISC --output-format concise -q
-:2:1: ISC001 [*] Implicitly concatenated string literals on one line
-:5:1: ISC002 Implicitly concatenated string literals over multiple lines
-:10:5: ISC003 [*] Explicitly concatenated string should be implicitly concatenated
```

### Version

ruff 0.12.12 (c6516e9b6 2025-09-04)

---

_Label `rule` added by @ntBre on 2025-09-08 21:22_

---

_Label `python314` added by @ntBre on 2025-09-08 21:22_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-09-09 12:11_

---

_Referenced in [astral-sh/ruff#20357](../../astral-sh/ruff/pulls/20357.md) on 2025-09-11 21:05_

---

_Closed by @dylwil3 on 2025-09-12 18:00_

---
