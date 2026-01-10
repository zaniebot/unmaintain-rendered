---
number: 19707
title: "RUF039 fix should be available for octal escapes and `u` string prefixes"
type: issue
state: open
author: dscorbett
labels:
  - fixes
  - preview
assignees: []
created_at: 2025-08-02T22:24:26Z
updated_at: 2025-08-05T13:27:38Z
url: https://github.com/astral-sh/ruff/issues/19707
synced_at: 2026-01-10T01:23:00Z
---

# RUF039 fix should be available for octal escapes and `u` string prefixes

---

_Issue opened by @dscorbett on 2025-08-02 22:24_

### Summary

The fix for [`unraw-re-pattern` (RUF039)](https://docs.astral.sh/ruff/rules/unraw-re-pattern/) should be available in two more cases.

If a backslash is followed by a zero, that is an octal escape sequence, not a backreference, so the fix should not be suppressed. [Example](https://play.ruff.rs/44a89d5a-8eb3-4c4d-add9-2c0de9e5f08a):
```console
$ cat >ruf039_1.py <<'# EOF'
import re
print(re.findall("\0|\07|\014", "\0\a\f"))
print(re.findall(b"\0|\07|\014", b"\0\a\f"))
# EOF

$ python ruf039_1.py
['\x00', '\x07', '\x0c']
[b'\x00', b'\x07', b'\x0c']

$ ruff --isolated check ruf039_1.py --select RUF039 --preview --output-format concise --unsafe-fixes --fix
ruf039_1.py:2:18: RUF039 First argument to `re.findall()` is not raw string
ruf039_1.py:3:18: RUF039 First argument to `re.findall()` is not raw bytes literal
Found 2 errors.
```

The ideal fixed output would be:
```python
import re
print(re.findall(r"\0|\07|\014", "\0\a\f"))
print(re.findall(rb"\0|\07|\014", b"\0\a\f"))
```

A `u` prefix on a pattern string has no effect so it should not block the fix. [Example](https://play.ruff.rs/14ce1c75-2640-49ee-bbd8-5c2a8f4b77fa):
```console
$ cat >ruf039_2.py <<'# EOF'
import re
print(re.match(u".", "!"))
# EOF

$ python ruf039_2.py
<re.Match object; span=(0, 1), match='!'>

$ ruff --isolated check ruf039_2.py --select RUF039 --preview --output-format concise --fix
ruf039_2.py:2:16: RUF039 First argument to `re.match()` is not raw string
Found 1 error.
```

The ideal fixed output would be:
```python
import re
print(re.match(r".", "!"))
```

### Version

ruff 0.12.7 (c5ac99889 2025-07-29)

---

_Label `fixes` added by @ntBre on 2025-08-05 13:27_

---

_Label `preview` added by @ntBre on 2025-08-05 13:27_

---
