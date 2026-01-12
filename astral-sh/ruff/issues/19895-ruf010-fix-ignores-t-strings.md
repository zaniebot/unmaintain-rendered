```yaml
number: 19895
title: RUF010 fix ignores t-strings
type: issue
state: closed
author: dscorbett
labels:
  - fixes
  - python314
assignees: []
created_at: 2025-08-13T13:33:20Z
updated_at: 2025-08-22T14:14:19Z
url: https://github.com/astral-sh/ruff/issues/19895
synced_at: 2026-01-12T15:54:57Z
```

# RUF010 fix ignores t-strings

---

_@dscorbett_

### Summary

The fix for [`explicit-f-string-type-conversion` (RUF010)](https://docs.astral.sh/ruff/rules/explicit-f-string-type-conversion/) ignores expressions containing t-strings. [Example](https://play.ruff.rs/9209c9fc-91f7-4333-b412-d433893bfb8d):
```console
$ cat >ruf010.py <<'# EOF'
f"{ascii(t"")}"
# EOF

$ ruff --isolated check ruf010.py --select RUF010 --target-version py314 --preview --fix --output-format concise
ruf010.py:1:4: RUF010 Use explicit conversion flag
Found 1 error.

$ cat ruf010.py
f"{ascii(t"")}"
```
Ideal fixed output:
```python
f"{t""!a}"
```

### Version

ruff 0.12.8 (f51a228f0 2025-08-07)

---

_Label `fixes` added by @ntBre on 2025-08-13 14:41_

---

_Label `python314` added by @ntBre on 2025-08-13 14:41_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-08-15 01:16_

---

_Comment by @dylwil3 on 2025-08-22 14:14_

This is a special case of https://github.com/astral-sh/ruff/issues/20043 and will automatically be resolved when that issue is resolved.

---

_Closed by @dylwil3 on 2025-08-22 14:14_

---
