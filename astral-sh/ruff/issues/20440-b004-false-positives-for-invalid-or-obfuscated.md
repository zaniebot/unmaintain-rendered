```yaml
number: 20440
title: "B004 false positives for invalid or obfuscated `hasattr` and `getattr` calls"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-09-16T19:31:23Z
updated_at: 2025-09-19T18:44:44Z
url: https://github.com/astral-sh/ruff/issues/20440
synced_at: 2026-01-12T15:54:57Z
```

# B004 false positives for invalid or obfuscated `hasattr` and `getattr` calls

---

_@dscorbett_

### Summary

[`unreliable-callable-check` (B004)](https://docs.astral.sh/ruff/rules/unreliable-callable-check/) doesn’t validate the arguments of `hasattr` or `getattr`.

It should be suppressed when `hasattr` is called with anything another than two positional non-starred arguments, lest it suppress or introduce an exception. [Example](https://play.ruff.rs/f11372d2-11f5-4800-a5ab-5fb7f39f86cb):
```console
$ cat >b004.py <<'# EOF'
try: print(hasattr(0, "__call__", 0, x=0))
except TypeError as e: print(e)

try: print(hasattr(*(), "__call__", "split"))
except TypeError as e: print(e)
# EOF

$ python b004.py
hasattr() takes no keyword arguments
True

$ ruff --isolated check b004.py --select B004 --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat b004.py
try: print(callable(0))
except TypeError as e: print(e)

try: print(callable(*()))
except TypeError as e: print(e)

$ python b004.py
False
callable() takes exactly one argument (0 given)
```

Similarly, B004 should only apply when `getattr` is called with two or three positional non-starred arguments.

### Version

ruff 0.13.0 (a1fdd66f1 2025-09-10)

---

_Label `bug` added by @ntBre on 2025-09-16 19:48_

---

_Label `fixes` added by @ntBre on 2025-09-16 19:48_

---

_Comment by @TaKO8Ki on 2025-09-16 20:47_

I will work on this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-09-16 22:09_

---

_Closed by @dylwil3 on 2025-09-19 18:44_

---
