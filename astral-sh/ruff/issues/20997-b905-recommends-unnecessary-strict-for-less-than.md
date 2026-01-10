```yaml
number: 20997
title: "B905 recommends unnecessary `strict` for less than two iterables"
type: issue
state: closed
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2025-10-20T15:25:10Z
updated_at: 2025-10-20T23:35:34Z
url: https://github.com/astral-sh/ruff/issues/20997
synced_at: 2026-01-10T11:10:00Z
```

# B905 recommends unnecessary `strict` for less than two iterables

---

_Issue opened by @dscorbett on 2025-10-20 15:25_

### Summary

[`zip-without-explicit-strict` (B905)](https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/) reports diagnostics for `zip` calls with less than two positional arguments, which is unnecessary. `strict` can only make a difference when there are two or more positional arguments, or any starred positional arguments. Compare [`map-without-explicit-strict` (B912)](https://docs.astral.sh/ruff/rules/map-without-explicit-strict/), which is only triggered when there are two or more positional arguments. [Example](https://play.ruff.rs/df4826f8-0b25-4c2f-9834-cde37b0538b2):
```console
$ cat >b905.py <<'# EOF'
print(tuple(zip()))
print(tuple(zip("abc")))

print(tuple(map(lambda x: x, "abc")))
# EOF

$ python b905.py
()
(('a',), ('b',), ('c',))
('a', 'b', 'c')

$ ruff --isolated check b905.py --select B905,B912 --target-version py314 --preview --unsafe-fixes --diff
--- b905.py
+++ b905.py
@@ -1,4 +1,4 @@
-print(tuple(zip()))
-print(tuple(zip("abc")))
+print(tuple(zip(strict=False)))
+print(tuple(zip("abc", strict=False)))
 
 print(tuple(map(lambda x: x, "abc")))

Would fix 2 errors.
```

### Version

ruff 0.14.1 (2bffef596 2025-10-16)

---

_Comment by @dylwil3 on 2025-10-20 15:47_

I assume we should also add the starred argument caveat to `B912`?

---

_Comment by @dscorbett on 2025-10-20 15:56_

Yes, that would make sense.

---

_Label `rule` added by @dylwil3 on 2025-10-20 16:03_

---

_Closed by @dylwil3 on 2025-10-20 23:35_

---
